---
description: Create a multi-agent group chat using AutoPattern (no handoffs). The Group Chat Manager selects agents automatically based on their descriptions. Use when conversation flow is unpredictable or defining explicit routing rules would be overly complex.
---

You are creating an AG2 group chat workflow using AutoPattern -- fully LLM-driven agent selection with no handoffs.

## Instructions

1. Ask the user for:
   - What task the group needs to solve
   - How many agents and their specializations
   - Maximum conversation rounds (default: 15)

2. Create the group chat following this pattern:

### AutoPattern Group Chat

```python
from autogen import ConversableAgent, UserProxyAgent, LLMConfig
from autogen.agentchat import run_group_chat
from autogen.agentchat.group.patterns import AutoPattern

llm_config = LLMConfig({"api_type": "anthropic", "model": "claude-sonnet-4-6"})

# Each agent MUST have a description -- used by Group Chat Manager for routing
agent_a = ConversableAgent(
    name="agent_a",
    system_message="Your role instructions here...",
    description="When to select this agent -- used for routing decisions.",
    llm_config=llm_config,
)

agent_b = ConversableAgent(
    name="agent_b",
    system_message="Your role instructions here...",
    description="When to select this agent -- used for routing decisions.",
    llm_config=llm_config,
)

user = UserProxyAgent(
    name="user",
    code_execution_config=False,
)

# AutoPattern -- no handoffs, LLM picks next agent based on descriptions
pattern = AutoPattern(
    initial_agent=agent_a,
    agents=[agent_a, agent_b],
    group_manager_args={"llm_config": llm_config},
    user_agent=user,
)

result = run_group_chat(
    pattern=pattern,
    messages="Your task here",
    max_rounds=15,
)
result.process()
print(result.summary)
```

### Key Rules

- **AutoPattern requires no handoffs** -- the Group Chat Manager decides routing based on agent `description` fields
- Every agent MUST have a distinct `description` (not just `system_message`) -- this is what the Group Chat Manager uses for selection
- The `system_message` tells the agent how to behave; the `description` tells the manager when to select the agent
- Use `LLMConfig({...})` -- NOT a raw dict like `{"model": "..."}`
- Use `run_group_chat` with a pattern -- NOT `initiate_chat` or `GroupChatManager`
- Keep `max_rounds` reasonable (10-20)

### When to Use This Pattern

- Unpredictable conversation flow where any specialist might be needed
- Brainstorming or collaborative problem-solving
- When defining explicit routing rules would be overly complex
- Natural team collaboration where agents join when their expertise is relevant

### Example

See `examples/organic_team.py` for a complete project management team example.
