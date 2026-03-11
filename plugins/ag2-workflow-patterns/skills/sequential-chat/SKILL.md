---
description: Create a sequential chat workflow where an initiator agent runs through a queue of chats in order. Use for step-by-step pipelines like outline -> plan -> format, where each stage builds on the previous output.
---

You are creating an AG2 sequential chat workflow using `a_sequential_run`.

## Instructions

1. Ask the user for:
   - The pipeline stages and what each agent does
   - Input/output expectations for each stage
   - Whether stages need tools

2. Create the sequential chat following this pattern:

### Sequential Chat Pattern

```python
import asyncio
from autogen import ConversableAgent, LLMConfig

llm_config = LLMConfig({"api_type": "anthropic", "model": "claude-sonnet-4-6"})

# The initiator agent runs the sequential pipeline
initiator = ConversableAgent(
    name="initiator",
    llm_config=llm_config,
)

stage_1 = ConversableAgent(
    name="stage_1",
    system_message="Do the first step.",
    llm_config=llm_config,
)

stage_2 = ConversableAgent(
    name="stage_2",
    system_message="Do the second step.",
    llm_config=llm_config,
)

stage_3 = ConversableAgent(
    name="stage_3",
    system_message="Produce the final output.",
    llm_config=llm_config,
)


async def main():
    chat_queue = [
        {
            "recipient": stage_1,
            "message": "Initial task description",
            "max_turns": 1,
            "summary_method": "last_msg",
        },
        {
            "recipient": stage_2,
            "message": "Continue with this",
            "max_turns": 1,
            "summary_method": "last_msg",
        },
        {
            "recipient": stage_3,
            "message": "Produce the final result",
            "max_turns": 1,
            "summary_method": "last_msg",
        },
    ]

    responses = await initiator.a_sequential_run(chat_queue)

    for i, response in enumerate(responses):
        await response.process()
        print(f"Stage {i + 1}: {await response.summary}")


if __name__ == "__main__":
    asyncio.run(main())
```

### Key Rules

- Use `a_sequential_run` (async) -- NOT chained `initiate_chat` calls
- Call `.process()` to run the workflow, then use `.summary` to extract the result
- Use `max_turns=1` per stage for clean handoffs
- `summary_method="last_msg"` passes output forward through the pipeline
- The `message` in each queue entry can provide stage-specific instructions
- Use `LLMConfig({...})` -- NOT a raw dict like `{"model": "..."}`

### When to Use This Pattern

- Fixed processing pipelines (A -> B -> C)
- Each stage does one transformation and passes results forward
- Extract-transform-load (ETL) workflows
- Multi-step content creation (outline -> draft -> polish)

### Example

See `examples/lesson_plan.py` for a curriculum -> activities -> lesson plan pipeline.
