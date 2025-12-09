---
name: "mcp-maker"
displayName: "Build MCP servers with FastMCP"
description: "Build production-ready MCP servers in Python with FastMCP framework, live docs, and agent-optimized tool design patterns"
author: "Praveen Chamarthi"
keywords: ["mcp", "model-context-protocol", "server", "tools", "resources", "prompts", "stdio", "http", "streamable", "fastmcp"]
---

# MCP Server Maker Power

Build production-ready MCP servers in Python with FastMCP framework, access to live documentation and curated best practices.

## Overview

Model Context Protocol (MCP) is an open standard that enables AI applications to securely connect to external data sources and tools. It uses a client-server architecture with JSON-RPC messages.

This power provides:

- Live documentation lookup via MCP Docs server
- Curated quickstart patterns in steering files
- Best practices from official MCP and FastMCP documentation

## CRITICAL: Always Read Getting Started Guide

**BEFORE building any MCP server, read the getting-started.md steering file.** It contains correct API usage, proper patterns, and essential setup steps. Use `kiroPowers` with action="readSteering", powerName="mcp-maker", steeringFile="getting-started.md".

## Available Steering Files

- **getting-started.md** - Complete quickstart guide with minimal server template, three primitives, and configuration examples
- **tool-design.md** - Best practices for designing effective tools that agents can use efficiently (naming, responses, token efficiency)

## Available MCP Servers

### mcp-docs

**Package:** `mcp-server-builder`

Provides live documentation lookup for both MCP protocol (modelcontextprotocol.io) and FastMCP framework (gofastmcp.com).

1. **search_mcp_docs** - Search documentation with ranked results
   - Required: `query` (string) - Search query (e.g., "tool input schema", "lifespan management")
   - Optional: `k` (integer) - Max results (default: 5)
   - Optional: `source` ("mcp" | "fastmcp") - Filter by documentation source
   - Returns: List of documents with URL, title, score, and snippet

2. **fetch_mcp_doc** - Fetch full document content by URL
   - Required: `uri` (string) - Document URL from search results
   - Returns: Full document with URL, title, and complete content

## Tool Usage Examples

### Search Documentation

```javascript
usePower("mcp-maker", "mcp-docs", "search_mcp_docs", {
  "query": "FastMCP tool decorator async",
  "k": 3
})
// Returns: Top 3 relevant docs about async tools
```

### Fetch Full Documentation

```javascript
usePower("mcp-maker", "mcp-docs", "fetch_mcp_doc", {
  "uri": "https://gofastmcp.com/servers/tools"
})
// Returns: Complete tools documentation page
```

## Three Primitives

| Primitive | Control | Description | Use Case |
|-----------|---------|-------------|----------|
| **Tools** | Model-controlled | Functions LLM can call | API calls, actions, side effects |
| **Resources** | Application-controlled | Read-only data access | Configs, files, database schemas |
| **Prompts** | User-controlled | Reusable templates | Slash commands, guided workflows |

## Best Practices

### Server Development

- Use `uv` for Python project management and `uvx` for running servers
- Use type hints on all parameters - FastMCP uses them for JSON Schema
- Use `async def` for I/O-bound operations (network, files)
- Log to stderr, never stdout: `import logging; logging.info("msg")`
- Never `print()` in stdio servers - corrupts JSON-RPC messages
- Test with MCP Inspector: `mcp dev server.py`

### Tool Design (Critical for Agent Effectiveness)

For detailed patterns, read `tool-design.md` steering file.

| Do | Don't |
|----|-------|
| Build consolidated workflow tools | Wrap every API endpoint separately |
| `search_contacts(query)` | `list_contacts()` (brute-force) |
| Return `name`, `title`, `status` | Return `uuid`, `mime_type` |
| Use `user_id` parameter name | Use ambiguous `user` parameter |
| Namespace: `github_create_issue` | Generic: `create_issue` |
| Add `format` param for response control | Always return full details |
| Provide actionable error messages | Return opaque error codes |

### Tool Descriptions

Write descriptions as if onboarding a new team member:

```python
@mcp.tool()
def search_orders(
    customer_id: str,
    status: Literal["pending", "shipped", "delivered"] | None = None,
    limit: int = 25
) -> list[dict]:
    """Search customer orders with optional filtering.

    Args:
        customer_id: Customer's unique identifier
        status: Filter by order status (optional)
        limit: Max results (default: 25, max: 100)

    Returns:
        List of orders with id, date, status, total, items_count
    """
```

### Token Efficiency

- Implement pagination with `limit`/`offset` parameters
- Add `format` parameter: "concise" vs "detailed"
- Truncate large responses with clear indicators
- Default limit: 25 results (Claude Code caps at 25,000 tokens)

## Quick Setup

```bash
# Create project
uv init my-mcp-server && cd my-mcp-server
uv add "mcp[cli]"

# activate venv
uv venv && source .venv/bin/activate

# Create server.py with @mcp.tool() decorated functions

# Test with Inspector
uv run mcp dev server.py

# Install in Claude Desktop
uv run mcp install server.py --name "My Server"
```

## Troubleshooting

### Server not connecting

1. Check Python version: `python --version` (3.10+ required)
2. Verify MCP CLI: `uv run mcp --help`
3. Check config path is absolute in claude_desktop_config.json
4. Review logs: `tail -f ~/Library/Logs/Claude/mcp*.log`

### Tool calls failing

1. Ensure all parameters have type hints
2. Check docstring format (Args section for descriptions)
3. Verify async functions use `await` for I/O
4. Test tool independently before adding to server

### JSON-RPC errors

1. Remove all `print()` statements
2. Use `logging` module with stderr handler
3. Check for accidental stdout writes in dependencies

### Import errors

1. Run `uv add <package>` for missing dependencies
2. Verify virtual environment is active
3. Check package is installed: `uv pip list`

## When to Load Steering Files

- Building your first MCP server → `getting-started.md`
- Designing tools for agent effectiveness → `tool-design.md`

## When to Use Live Docs

Use `search_mcp_docs` for:

- Advanced patterns (lifespan, context, images)
- Transport options (stdio, SSE, streamable HTTP)
- Low-level Server API
- Client implementation
- Protocol specification details
- Resource templates and subscriptions
- Prompt argument handling

---

**Package:** `mcp-server-builder`
**Documentation:** modelcontextprotocol.io, gofastmcp.com
