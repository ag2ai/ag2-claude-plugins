---
description: Create a simple two-agent chat where two agents converse back and forth. The simplest multi-agent pattern -- use for iterative refinement, Q&A, debate, or any task where two roles alternate.
---

You are creating an AG2 two-agent chat -- the simplest multi-agent pattern.

## Instructions

1. Ask the user for:
   - What each agent's role is
   - How many turns of conversation (default: 2-4)
   - Whether to summarize the result

2. Create the two-agent chat following this pattern:

### Two-Agent Chat Pattern

```python
import asyncio
from autogen import ConversableAgent, LLMConfig

llm_config = LLMConfig({"api_type": "anthropic", "model": "claude-sonnet-4-6"})

agent_a = ConversableAgent(
    name="agent_a",
    system_message="Your role and behavior instructions.",
    llm_config=llm_config,
)

agent_b = ConversableAgent(
    name="agent_b",
    system_message="Your role and behavior instructions.",
    llm_config=llm_config,
)


async def main():
    response = await agent_a.a_run(
        agent_b,
        message="Your task or question here",
        max_turns=2,
        summary_method="reflection_with_llm",
    )
    await response.process()
    print(await response.summary)


if __name__ == "__main__":
    asyncio.run(main())
```

### Key Rules

- Use `a_run` (async) with `.process()` then `.summary` -- NOT `initiate_chat`
- `max_turns` controls conversation rounds (each turn = both agents speak)
- `summary_method="reflection_with_llm"` generates a summary; use `"last_msg"` for the raw last message
- Use `LLMConfig({...})` -- NOT a raw dict like `{"model": "..."}`
- You can use different models per agent (e.g., fast model for one, capable for the other)

### Common Patterns

- **Creator + Reviewer**: Draft content, get feedback, revise
- **Student + Teacher**: Ask questions, get explanations
- **Interviewer + Expert**: Deep-dive into a topic
- **Debater A + Debater B**: Explore both sides of an argument

### Example

See `examples/student_teacher.py` for a student-teacher Q&A conversation.
