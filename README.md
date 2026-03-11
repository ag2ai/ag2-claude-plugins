# AG2 Agent Builder - Claude Code Plugin Marketplace

A Claude Code plugin marketplace for building [AG2 (AutoGen)](https://github.com/ag2ai/ag2) based agents, multi-agent workflows, and A2A services.

## Installation

```bash
# In Claude Code
/plugin marketplace add ag2ai/ag2-claude-plugins
```

Or for local development:
```bash
/plugin marketplace add /path/to/ag2-claude-plugins
```

Then install individual plugins from the Discover tab:
```bash
/plugin
```

## Plugins

### ag2-agent-scaffold

Scaffold new AG2 agents with proper structure, tool functions, and A2A server wiring.

| Skill | Description |
|-------|-------------|
| `/new-agent` | Create a ConversableAgent with tools, system prompt, and LLM config |
| `/new-a2a-agent` | Create a full A2A-compliant agent with server wiring and CardSettings |

### ag2-workflow-patterns

Multi-agent orchestration patterns for common use cases.

| Skill | Description |
|-------|-------------|
| `/ag2-chat-fundamentals` | Learn AG2 chat fundamentals and ConversableAgent basics |
| `/two-agent-chat` | Set up a two-agent conversation pattern |
| `/sequential-chat` | Create a sequential multi-agent pipeline |
| `/group-chat` | Set up a multi-agent group chat with configurable speaker selection |
| `/group-chat-auto-pattern` | Group chat with automatic speaker selection pattern |
| `/nested-chat` | Create nested chat workflows (hub-and-spoke) |

### ag2-tool-builder

Create well-structured tool functions with proper contracts.

| Skill | Description |
|-------|-------------|
| `/new-tool` | Generate a tool function with type annotations, docstrings, and JSON contracts |
| `/connector-tool` | Build API connector tools with auth handling, pagination, and error mapping |

### ag2-agent-reviewer

Review AG2 agent code for common issues.

| Agent | Description |
|-------|-------------|
| `ag2-reviewer` | Analyzes agent code for tool contract violations, prompt quality, security, and best practices |

### ag2-agent-guide

Architecture advisors for AG2 agent systems.

| Agent | Description |
|-------|-------------|
| `ag2-architect` | Recommends orchestration patterns (group chat, pipeline, handoffs, etc.) based on your use case |
| `ag2-prompt-engineer` | Crafts and reviews system prompts for maximum agent reliability |

## Pattern Quick Reference

| Need | Pattern | Plugin |
|------|---------|--------|
| Single agent with API access | Tool-augmented agent | ag2-agent-scaffold |
| Draft + review cycle | Two-agent chat | ag2-workflow-patterns |
| Step-by-step processing | Sequential pipeline | ag2-workflow-patterns |
| Collaborative problem solving | Group chat | ag2-workflow-patterns |
| Expert consultation | Nested chats (hub-and-spoke) | ag2-workflow-patterns |
| Context-dependent routing | Group chat (DefaultPattern with handoffs) | ag2-workflow-patterns |
| Deployable A2A service | A2A agent server | ag2-agent-scaffold |

## AG2 Resources

- [AG2 Documentation](https://docs.ag2.ai/)
- [AG2 GitHub](https://github.com/ag2ai/ag2)
- [A2A Protocol](https://github.com/google/A2A)

## Contributing

1. Fork the repository
2. Create a feature branch
3. Add or modify plugins under `plugins/`
4. Test locally with `/plugin marketplace add ./ag2-claude-plugins`
5. Submit a pull request

### Adding a New Plugin

1. Create a directory under `plugins/<plugin-name>/`
2. Add `.claude-plugin/plugin.json` with name, version, description
3. Add skills (`skills/<name>/SKILL.md`) or agents (`agents/<name>.md`)
4. Add the plugin entry to `.claude-plugin/marketplace.json`
5. Test with `/plugin marketplace add .` and verify your skills/agents appear

## License

Apache-2.0
