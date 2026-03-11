---
description: Create a new tool function for an AG2 agent. Generates properly typed, documented tool functions with JSON return contracts and error handling.
---

You are creating tool functions for AG2 agents. Tools are how agents interact with external systems.

## Instructions

1. Ask the user for:
   - What the tool does (API call, file operation, computation, etc.)
   - Input parameters and their types
   - Expected output structure
   - Error scenarios to handle

2. Generate the tool following this exact contract:

### Tool Function Pattern

```python
import json
from autogen.tools import tool


@tool()
def tool_name(required_param: str, optional_param: int = 10) -> str:
    """Clear, specific description of what this tool does.

    The docstring becomes the function description in the LLM's tool schema.
    Be specific about what the tool returns and when to use it.

    Args:
        required_param: What this parameter is for
        optional_param: What this controls (default: 10)
    """
    try:
        # Implementation here
        result = {
            "items": [],
            "total_count": 0,
            "metadata": {},
        }
        return json.dumps({"success": True, "data": result})
    except ValueError as e:
        return json.dumps({"success": False, "error": f"Invalid input: {e}"})
    except Exception as e:
        return json.dumps({"success": False, "error": str(e)})
```

### Tool Contract Rules

**Return types**: Tool functions can return:
- `str` -- plain text or JSON strings
- `pydantic.BaseModel` -- automatically serialized by the framework
- `ReplyResult` -- for group chat tool-based handoffs (includes `target` for routing)

The JSON `{"success": true, "data": ...}` pattern shown above is a good convention for structured tools but is NOT required.

**Type annotations**: All parameters MUST have type annotations. The LLM uses these to construct calls.

**Docstrings**: MUST include:
- One-line summary of what the tool does
- `Args:` section describing each parameter
- Be specific -- vague descriptions lead to misuse by the LLM

**Parameter design**:
- Use simple types: `str`, `int`, `float`, `bool`
- Use `str` for complex inputs (JSON strings) -- avoid nested objects
- Provide sensible defaults for optional parameters
- Limit to 5 parameters max -- split into multiple tools if more needed

**Error handling**:
- The AG2 framework catches all uncaught tool exceptions automatically and converts them to `"Error: {message}"` strings
- Catching exceptions in tools is good practice for custom error messages but NOT required
- Error messages should help the LLM retry or explain to the user
- For missing credentials: return `{"success": false, "error": "connector_setup_required:<connector_id>"}`

### Anti-Patterns to Avoid

```python
# BAD: No type annotations
@tool()
def bad_tool(x):
    return json.dumps({"success": True})

# BAD: No docstring
@tool()
def bad_tool(x: str) -> str:
    return json.dumps({"success": True})

# BAD: Vague docstring
@tool()
def bad_tool(x: str) -> str:
    """Does stuff."""  # LLM won't know when to use this
    return json.dumps({"success": True})
```

3. After generating, verify: type annotations, docstring quality, error handling, and JSON return format.
