# Building MCP Servers with FastMCP

Quick reference for building MCP servers in Python. Use the MCP Docs server (`search_mcp_docs`, `fetch_mcp_doc`) for detailed documentation.

## Quickstart: Build Your First Server

**1. Validate dependencies:**

```bash
# Python 3.10+ required
python --version

# Install uv if needed
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**2. Create project:**

```bash
uv init my-mcp-server
cd my-mcp-server
uv add "mcp[cli]"
uv add httpx  # For HTTP clients (optional)
# craete and activate venv
uv venv && source .venv/bin/activate
```

**3. Create server** (`server.py`):

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("my-server")

@mcp.tool()
def hello(name: str) -> str:
    """Greet someone by name.

    Args:
        name: The person's name
    """
    return f"Hello, {name}!"

@mcp.resource("config://app")
def get_config() -> str:
    """Return application configuration."""
    return "config data here"

@mcp.prompt()
def review_code(code: str) -> str:
    """Create a code review prompt."""
    return f"Please review this code:\n\n{code}"

if __name__ == "__main__":
    mcp.run(transport='stdio')
```

**4. Test immediately:**

```bash
# Development with MCP Inspector
uv run mcp dev server.py

# Direct execution
uv run python server.py
```

## Three Primitives

| Primitive | Control | Decorator | Use Case |
|-----------|---------|-----------|----------|
| **Tools** | Model | `@mcp.tool()` | Actions, API calls, side effects |
| **Resources** | Application | `@mcp.resource("uri://...")` | Read-only data, configs |
| **Prompts** | User | `@mcp.prompt()` | Templates, slash commands |

## Async Tools (for I/O)

```python
import httpx

@mcp.tool()
async def fetch_data(url: str) -> str:
    """Fetch data from a URL."""
    async with httpx.AsyncClient() as client:
        response = await client.get(url, timeout=30.0)
        response.raise_for_status()
        return response.text
```

## Dynamic Resources

```python
@mcp.resource("users://{user_id}/profile")
def get_user(user_id: str) -> str:
    """Get user profile by ID."""
    return f"Profile for {user_id}"
```

## Running Your Server

```bash
# Development with Inspector UI
uv run mcp dev server.py

# Install in Claude Desktop
uv run mcp install server.py --name "My Server"

# Direct execution
uv run python server.py
```

## Claude Desktop / Kiro Configuration

```json
{
  "mcpServers": {
    "my-server": {
      "command": "uv",
      "args": ["--directory", "/absolute/path/to/project", "run", "server.py"]
    }
  }
}
```

With environment variables:

```json
{
  "mcpServers": {
    "my-server": {
      "command": "uv",
      "args": ["--directory", "/path/to/project", "run", "server.py"],
      "env": {
        "API_KEY": "your-api-key"
      }
    }
  }
}
```

## Key Rules

1. **Never print to stdout** - Corrupts JSON-RPC messages in stdio transport

   ```python
   # ❌ Bad
   print("Processing")

   # ✅ Good
   import logging
   logging.info("Processing")
   ```

2. **Type hints required** - FastMCP uses them for JSON Schema generation

3. **Docstrings become descriptions** - Write clear tool/resource descriptions

4. **Use async for I/O** - Network calls, file operations should be async

5. **Validate inputs** - Return clear error messages for invalid inputs

## Error Handling

```python
@mcp.tool()
def divide(a: float, b: float) -> float:
    """Divide two numbers."""
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
```

## Next Steps

Use the MCP Docs server for advanced topics:

```text
search_mcp_docs("lifespan context management")
search_mcp_docs("progress reporting")
search_mcp_docs("streamable HTTP transport")
search_mcp_docs("low-level Server API")
search_mcp_docs("MCP client implementation")
```
