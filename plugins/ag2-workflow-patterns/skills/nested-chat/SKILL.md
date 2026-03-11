---
description: Create a nested chat workflow where a multi-step pipeline is encapsulated inside a single agent. Use when you want to package a sequence of agent interactions (research, draft, edit) into one agent that runs the pipeline automatically when triggered.
---

You are creating an AG2 nested chat workflow. This encapsulates a multi-step pipeline inside a single agent using `register_nested_chats`.

## Instructions

1. Ask the user for:
   - The pipeline stages and what each agent does
   - The trigger condition (which sender activates the nested pipeline)
   - How results pass between stages (`summary_method`)

2. Create the nested chat following this pattern:

### Nested Chat Pattern

```python
import asyncio
from autogen import ConversableAgent, LLMConfig

llm_config = LLMConfig({"api_type": "anthropic", "model": "claude-sonnet-4-6"})

user = ConversableAgent(
    name="user",
    human_input_mode="NEVER",
)

# The outer agent that encapsulates the pipeline
coordinator = ConversableAgent(
    name="coordinator",
    system_message="Present the final result.",
    llm_config=llm_config,
)

# Pipeline stage agents
step_1 = ConversableAgent(
    name="step_1",
    system_message="Do the first step.",
    llm_config=llm_config,
)

step_2 = ConversableAgent(
    name="step_2",
    system_message="Do the second step.",
    llm_config=llm_config,
)

step_3 = ConversableAgent(
    name="step_3",
    system_message="Do the third step.",
    llm_config=llm_config,
)

# Register the nested pipeline -- fires when coordinator receives from user
coordinator.register_nested_chats(
    chat_queue=[
        {
            "recipient": step_1,
            "message": lambda recipient, messages, sender, config: messages[-1]["content"],
            "max_turns": 1,
            "summary_method": "last_msg",
        },
        {
            "recipient": step_2,
            "message": "Continue with the second step.",
            "max_turns": 1,
            "summary_method": "last_msg",
        },
        {
            "recipient": step_3,
            "message": "Complete the third step.",
            "max_turns": 1,
            "summary_method": "last_msg",
        },
    ],
    trigger=user,  # fires when message comes from user
)


async def main():
    response = await user.a_run(
        coordinator,
        message="Your task here",
        max_turns=1,
    )
    await response.process()
    print(await response.summary)


if __name__ == "__main__":
    asyncio.run(main())
```

### Key Rules

- Use `register_nested_chats` to define the pipeline on the outer agent
- Each chat in `chat_queue` MUST have a `message` field -- without it, subsequent stages won't fire
- The first stage should use a callable `message` to forward the original user request: `lambda recipient, messages, sender, config: messages[-1]["content"]`
- Subsequent stages can use static `message` strings -- the previous stage's output is automatically appended as context
- Each chat in `chat_queue` runs sequentially -- output of one feeds into the next
- Use `max_turns=1` per stage for clean handoffs
- `summary_method="last_msg"` passes the last message as input to the next stage
- The `trigger` parameter controls which sender activates the nested pipeline
- Use `a_run` (async) with `.process()` then `.summary` -- NOT `initiate_chat`
- Use `LLMConfig({...})` -- NOT a raw dict like `{"model": "..."}`

### When to Use This Pattern

- Packaging a complex workflow into a single agent interface
- When the pipeline should be invisible to the caller
- Multi-step processing: research -> draft -> review -> polish
- Hub-and-spoke where a coordinator consults multiple specialists

### Example

See `examples/article_pipeline.py` for a research-draft-edit pipeline.
