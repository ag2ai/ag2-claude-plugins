---
description: Create a multi-agent group chat using DefaultPattern with explicit handoffs. Use when you need deterministic control over agent transitions, pipelines with validation gates, hierarchical delegation, or context-aware routing between specialists.
---

You are creating an AG2 group chat workflow using DefaultPattern -- explicit handoffs control agent transitions.

## Instructions

1. Ask the user for:
   - What task the group needs to solve
   - How many agents and their specializations
   - The handoff/routing logic between agents
   - Whether agents need tools (functions)
   - Whether shared context variables are needed

2. Create the group chat following this pattern:

### DefaultPattern Group Chat

```python
from autogen import ConversableAgent, UserProxyAgent, LLMConfig
from autogen.agentchat import run_group_chat
from autogen.agentchat.group.patterns import DefaultPattern
from autogen.agentchat.group import (
    AgentTarget,
    AgentNameTarget,
    OnCondition,
    StringLLMCondition,
    OnContextCondition,
    ContextExpression,
    ExpressionContextCondition,
    ReplyResult,
    ContextVariables,
    RevertToUserTarget,
    TerminateTarget,
)

llm_config = LLMConfig({"api_type": "anthropic", "model": "claude-sonnet-4-6"})

# Shared context for tracking state across agents
shared_context = ContextVariables(data={
    "stage_completed": False,
})

# Tool function that controls handoff via ReplyResult
def process_task(result: str, context_variables: ContextVariables) -> ReplyResult:
    """Process and hand off to next agent"""
    context_variables["stage_completed"] = True
    return ReplyResult(
        message=f"Task processed: {result}",
        context_variables=context_variables,
        target=AgentNameTarget("next_agent"),  # Explicit handoff
    )

agent_a = ConversableAgent(
    name="agent_a",
    system_message="Your role instructions...",
    functions=[process_task],
    llm_config=llm_config,
)

agent_b = ConversableAgent(
    name="next_agent",
    system_message="Your role instructions...",
    llm_config=llm_config,
)

user = UserProxyAgent(name="user", code_execution_config=False)

# Register handoffs (see Handoffs section below)
agent_a.handoffs.add_context_condition(
    OnContextCondition(
        target=AgentTarget(agent_b),
        condition=ExpressionContextCondition(
            ContextExpression("${stage_completed} == True")
        ),
    ),
)
agent_a.handoffs.set_after_work(RevertToUserTarget())

pattern = DefaultPattern(
    initial_agent=agent_a,
    agents=[agent_a, agent_b],
    user_agent=user,
    context_variables=shared_context,
)

result = run_group_chat(
    pattern=pattern,
    messages="Your task here",
    max_rounds=30,
)
result.process()
print(result.summary)
# result.context_variables has the final shared state
# result.last_speaker has the last agent's name
```

---

## Context Variables

Context Variables provide shared memory across all agents in a group chat. They persist through handoffs and are **intentionally separate from LLM prompts** for token efficiency and security.

### Creating and Using Context Variables

```python
from autogen.agentchat.group import ContextVariables

context = ContextVariables(data={
    "user_name": "Alex",
    "issue_count": 0,
    "previous_issues": [],
})

# Reading
user_name = context["user_name"]
user_name = context.get("user_name", "default")

# Writing
context["issue_count"] = 1
context.set("issue_count", 1)

# Bulk update
context.update({"last_login": "2025-01-01", "premium": True})

# Checking
if "premium" in context:
    print("Premium user")
```

### Three Ways Agents Access Context Variables

**1. Tool functions** -- `context_variables` parameter is auto-injected:

```python
def check_history(query: str, context_variables: ContextVariables) -> str:
    """Check user's previous issues."""
    user_name = context_variables.get("user_name", "User")
    issue_count = context_variables.get("issue_count", 0)
    return f"User {user_name} has {issue_count} previous issues."
```

Tools returning `ReplyResult` can update context AND hand off:

```python
def route_to_tech(issue: str, context_variables: ContextVariables) -> ReplyResult:
    """Route to tech support and update context."""
    context_variables["current_issue"] = issue
    context_variables["issue_count"] += 1
    return ReplyResult(
        message="Routing to tech support...",
        target=AgentTarget(tech_agent),
        context_variables=context_variables,
    )
```

**2. UpdateSystemMessage** -- inject context into the system prompt dynamically:

```python
from autogen import UpdateSystemMessage

agent = ConversableAgent(
    name="support_agent",
    system_message="You are a helpful support agent.",
    update_agent_state_before_reply=[
        UpdateSystemMessage(
            "You are helping {user_name} (Premium: {is_premium}). "
            "They have reported {issue_count} issues. "
            "Current issue type: {issue_type}"
        )
    ],
    llm_config=llm_config,
)
```

**3. Context summary tools** -- let the agent query session state on demand:

```python
def get_session_summary(context_variables: ContextVariables) -> str:
    """Get a summary of the current support session."""
    return (
        f"User: {context_variables.get('user_name', 'Unknown')}\n"
        f"Issues: {context_variables.get('issue_count', 0)}\n"
        f"Status: {context_variables.get('status', 'Active')}"
    )
```

---

## Handoffs

Handoffs control how agents transition to each other. There are four types, plus transition targets.

### Transition Targets

| Target | Description |
|--------|-------------|
| `AgentTarget(agent)` | Transfer to a specific agent instance |
| `AgentNameTarget("name")` | Transfer by agent name string |
| `RevertToUserTarget()` | Return control to user |
| `TerminateTarget()` | End the conversation |
| `GroupManagerTarget()` | Delegate to group chat manager |
| `NestedChatTarget(...)` | Start a nested chat |
| `RandomAgentTarget([...])` | Random selection from agent list |
| `StayTarget()` | Keep current agent |
| `AskUserTarget()` | Ask user to select next agent |

### 1. Context-Based Handoffs (deterministic, no LLM cost)

Route based on context variable state. Prefer this over LLM-based when possible.

```python
from autogen.agentchat.group import (
    OnContextCondition, ExpressionContextCondition, ContextExpression,
    StringContextCondition,
)

# Expression-based: complex conditions
agent.handoffs.add_context_condition(
    OnContextCondition(
        target=AgentTarget(escalation_agent),
        condition=ExpressionContextCondition(
            ContextExpression("${issue_count} >= 3 and ${is_premium} == True")
        ),
    )
)

# Simple truthy check on a single variable
agent.handoffs.add_context_condition(
    OnContextCondition(
        target=AgentTarget(order_agent),
        condition=StringContextCondition(variable_name="logged_in"),
    )
)
```

### 2. LLM-Based Handoffs (flexible, uses LLM reasoning)

Route based on LLM evaluation of conversation context.

```python
from autogen.agentchat.group import OnCondition, StringLLMCondition

tech_agent.handoffs.add_llm_conditions([
    OnCondition(
        target=AgentTarget(computer_agent),
        condition=StringLLMCondition(
            prompt="Route when issue involves laptops, desktops, PCs, or Macs."
        ),
    ),
    OnCondition(
        target=AgentTarget(smartphone_agent),
        condition=StringLLMCondition(
            prompt="Route when issue involves phones or mobile devices."
        ),
    ),
])
```

LLM conditions with context variable substitution:

```python
from autogen.agentchat.group import ContextStrLLMCondition, ContextStr

agent.handoffs.add_llm_condition(
    OnCondition(
        target=AgentTarget(tech_support),
        condition=ContextStrLLMCondition(
            ContextStr(
                "Transfer to tech support if technical issue. "
                "Current user: {user_name}, Issue count: {issue_count}"
            )
        ),
    )
)
```

### 3. After-Work Behavior (default fallback)

What happens when no handoff condition triggers. Agent-level overrides pattern-level.

```python
# Agent-level
tech_agent.handoffs.set_after_work(RevertToUserTarget())

# Pattern-level default for all agents
pattern = DefaultPattern(
    initial_agent=triage_agent,
    agents=[triage_agent, tech_agent],
    user_agent=user,
    group_after_work=RevertToUserTarget(),  # Default for all agents
)

# Agent-level overrides pattern-level
tech_agent.handoffs.set_after_work(TerminateTarget())  # This wins for tech_agent
```

### 4. Tool-Based Handoffs (handoff as side-effect of tool execution)

Tools return `ReplyResult` specifying the next agent.

```python
def classify_query(query: str, context_variables: ContextVariables) -> ReplyResult:
    """Classify and route a query."""
    technical_keywords = ["error", "bug", "crash", "broken"]
    if any(kw in query.lower() for kw in technical_keywords):
        return ReplyResult(
            message="Technical issue detected.",
            target=AgentTarget(tech_agent),
            context_variables=context_variables,
        )
    return ReplyResult(
        message="General question.",
        target=AgentTarget(general_agent),
        context_variables=context_variables,
    )
```

### Conditional Availability

Make handoffs only available when conditions are met:

```python
from autogen.agentchat.group import StringAvailableCondition, ExpressionAvailableCondition

OnCondition(
    target=AgentTarget(auth_agent),
    condition=StringLLMCondition("Transfer if user needs to log in"),
    available=StringAvailableCondition(context_variable="requires_login"),
)

OnContextCondition(
    target=AgentTarget(specialist),
    condition=ExpressionContextCondition(ContextExpression("not(${task_completed})")),
    available=ExpressionAvailableCondition(ContextExpression("${task_started} == True")),
)
```

---

## Key Rules

- Use `DefaultPattern` with explicit handoffs -- NOT `AutoPattern` (that's for the auto pattern)
- Use `LLMConfig({...})` -- NOT a raw dict like `{"model": "..."}`
- Use `run_group_chat` with a pattern -- NOT `initiate_chat` or `GroupChatManager`
- Use `ContextVariables` for shared state across agents
- Use `ReplyResult` with `target=` for tool-driven handoffs
- Use `RevertToUserTarget()` to return control to the user
- Prefer `OnContextCondition` (deterministic, no LLM cost) over `OnCondition` (LLM-based) when possible
- Agent-level `set_after_work` overrides pattern-level `group_after_work`
- Context variables are NOT automatically visible to the LLM -- use `UpdateSystemMessage` or tools to expose them

## Common Patterns (see examples/)

- **Pipeline**: Linear sequence (A -> B -> C) with validation gates
- **Star/Hub-and-Spoke**: Coordinator delegates to specialists and synthesizes results
- **Hierarchical/Tree**: Executive -> Managers -> Specialists with structured reporting
- **Context-Aware Routing**: Dynamic routing based on content analysis
- **Escalation**: Progressive capability tiers (basic -> intermediate -> advanced)
- **Feedback Loop**: Iterative refinement with quality gates
- **Redundant**: Multiple agents tackle same task, evaluator picks best
- **Triage with Tasks**: Decompose complex requests into categorized task sequences
