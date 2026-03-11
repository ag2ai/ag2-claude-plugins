---
description: Core AG2 conventions that apply to all agent patterns -- LLMConfig, ending chats, agent types, and common APIs. Loaded automatically when building any AG2 workflow.
user-invocable: false
---

# AG2 Chat Fundamentals

These conventions apply to ALL AG2 agent patterns (two-agent chat, sequential chat, nested chat, group chat).

## References

- **AG2 GitHub**: https://github.com/ag2ai/ag2
- **AG2 Docs**: https://docs.ag2.ai/latest/
- **API Reference**: https://docs.ag2.ai/latest/docs/api-reference/autogen/Agent/

## LLM Configuration

Always use `LLMConfig` -- never pass a raw dict as `llm_config`.

```python
from autogen import LLMConfig

# Correct -- LLMConfig wrapping a dict
llm_config = LLMConfig({"api_type": "anthropic", "model": "claude-sonnet-4-6"})

# Wrong -- raw dict
# llm_config = {"model": "claude-sonnet-4-6"}
```

## Agent Naming

Agent names must **never contain spaces**. Use lowercase with underscores for multi-word names:

```python
# Correct
name="project_manager"
name="qa_engineer"

# Wrong
name="Project Manager"
name="QA Engineer"
```

## Agent `description` Field

The `description` parameter is **critical for AutoPattern group chats** -- the underlying Group Chat Manager uses it to decide which agent to select next. Every agent in an AutoPattern group chat MUST have a distinct `description` explaining when to select that agent. The `system_message` tells the agent how to behave; the `description` tells the manager when to route to that agent.

For non-group-chat patterns (two-agent, sequential, nested), `description` is optional.

## Agent Types

- **`ConversableAgent`**: The base agent. Use for most agents. Defaults: `human_input_mode="TERMINATE"`, `code_execution_config=False`.
- **`UserProxyAgent`**: A human-in-the-loop proxy. Defaults: `human_input_mode="ALWAYS"`, `code_execution_config={}` (enabled). If you want `human_input_mode="NEVER"` with no code execution, just use `ConversableAgent` instead.

## Ending a Chat

There are several ways agent conversations end in AG2. These apply to two-agent chats, sequential chats, and group chats.

### 1. Maximum Turns / Rounds

For two-agent chats, use `max_turns` on `a_run`:

```python
response = await agent_a.a_run(
    agent_b,
    message="Your task",
    max_turns=3,
)
```

For group chats, use `max_rounds` on `run_group_chat`:

```python
result = run_group_chat(
    pattern=pattern,
    messages="Your task",
    max_rounds=15,
)
result.process()
print(result.summary)
```

### 2. TerminateTarget (Group Chat)

An agent hands off to `TerminateTarget()` to explicitly end a group chat.

```python
from autogen.agentchat.group import TerminateTarget, ReplyResult

# As after-work behavior
final_agent.handoffs.set_after_work(TerminateTarget())

# From a tool via ReplyResult
def finish_task(summary: str, context_variables: ContextVariables) -> ReplyResult:
    """Complete the task and end the conversation."""
    context_variables["final_summary"] = summary
    return ReplyResult(
        message=f"Task complete: {summary}",
        context_variables=context_variables,
        target=TerminateTarget(),
    )
```

### 3. RevertToUserTarget (Group Chat)

Returns control to the user agent. If no `user_agent` is configured in the pattern, this ends the conversation.

```python
from autogen.agentchat.group import RevertToUserTarget

agent.handoffs.set_after_work(RevertToUserTarget())
```

### 4. Termination Message

An agent configured with `is_termination_msg` ends the chat when it receives a matching message.

```python
agent = ConversableAgent(
    name="agent",
    is_termination_msg=lambda msg: "TERMINATE" in (msg.get("content", "") or ""),
    llm_config=llm_config,
)
```

### 5. Human Exit

When `human_input_mode="ALWAYS"` (the `UserProxyAgent` default), the user can type `exit` to end the conversation immediately.

### 6. Max Consecutive Auto Reply

Limits how many times an agent can reply consecutively before stopping.

```python
agent = ConversableAgent(
    name="agent",
    max_consecutive_auto_reply=3,
    llm_config=llm_config,
)
```

## Chat APIs

Use the modern async APIs:

| Pattern | API | Description |
|---------|-----|-------------|
| Two-agent | `await agent_a.a_run(agent_b, message=..., max_turns=...)` | Two agents converse |
| Sequential | `await initiator.a_sequential_run(chat_queue)` | Chain of chats in order |
| Nested | `agent.register_nested_chats(chat_queue, trigger=...)` | Pipeline inside one agent |
| Group chat | `run_group_chat(pattern=..., messages=..., max_rounds=...)` | Multi-agent with patterns |

Call `.process()` to run the workflow, then access `.summary` for the result:

```python
response = await agent_a.a_run(agent_b, message="Hello", max_turns=2)
await response.process()        # processes events, returns None
print(await response.summary)   # async property -- must await
```

## Summary Methods

When passing output between stages, use `summary_method`:

- `"last_msg"` -- pass the last message as-is (default for pipelines)
- `"reflection_with_llm"` -- use an LLM to summarize the conversation
