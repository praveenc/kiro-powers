# MCP Maker - Kiro Power

Build production-ready MCP servers in Python with FastMCP framework, live docs, and agent-optimized tool design patterns.

## Overview

**MCP Maker** is a Kiro Power that helps you build Model Context Protocol (MCP) servers using Python and the FastMCP framework. It provides:

- **Live documentation lookup** - Search and fetch docs from modelcontextprotocol.io and gofastmcp.com
- **Curated quickstart patterns** - Minimal server templates and best practices
- **Agent-optimized tool design** - Learn how to build tools that AI agents can use effectively

## What's Included

### MCP Servers
- **mcp-server-builder** - Live documentation search and retrieval for MCP protocol and FastMCP framework

### Steering Files
- **getting-started.md** - Complete quickstart guide with minimal server template, three primitives, and configuration examples
- **tool-design.md** - Best practices for designing effective tools that agents can use efficiently (naming, responses, token efficiency)

## Installation

### In Kiro IDE

1. Open Kiro → Powers panel
2. Click **Add power from GitHub**
3. Enter: `https://github.com/praveenc/mcp-maker`
4. Click Install

### Manual Installation

```bash
git clone https://github.com/praveenc/mcp-maker.git
cd mcp-maker
```

Then in Kiro:
1. Open Powers panel
2. Click **Add power from Local Path**
3. Select the `mcp-maker` directory

## Quick Start

Once installed, mention MCP-related keywords in your Kiro chat:

```
"Help me build an MCP server"
"Create a FastMCP tool for searching contacts"
"How do I design tools for AI agents?"
```

Kiro will automatically activate the MCP Maker power and load relevant context.

## Key Features

### Three MCP Primitives

| Primitive | Control | Use Case |
|-----------|---------|----------|
| **Tools** | Model-controlled | API calls, actions, side effects |
| **Resources** | Application-controlled | Configs, files, database schemas |
| **Prompts** | User-controlled | Slash commands, guided workflows |

### Best Practices Included

- Use `uv` for Python project management
- Type hints for automatic JSON Schema generation
- Async/await for I/O-bound operations
- Proper logging (stderr, not stdout)
- Token-efficient tool design
- Agent-friendly naming conventions

## Example: Building Your First Server

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("my-server")

@mcp.tool()
def search_contacts(query: str, limit: int = 10) -> list[dict]:
    """Search contacts by name, email, or company.
    
    Args:
        query: Search term
        limit: Maximum results (default: 10)
    """
    # Your implementation here
    return results

if __name__ == "__main__":
    mcp.run(transport='stdio')
```

## Documentation

- [MCP Protocol Docs](https://modelcontextprotocol.io)
- [FastMCP Framework](https://gofastmcp.com)
- [Kiro Powers Guide](https://kiro.dev/docs/powers/)

## Requirements

- Python 3.10+
- `uv` package manager
- Kiro IDE

## Contributing

This power is maintained independently. For issues or suggestions, please open a GitHub issue.

## License

MIT License - See LICENSE file for details

## Author

Praveen Chamarthi

---

**Keywords:** mcp, model-context-protocol, server, tools, resources, prompts, stdio, http, streamable, fastmcp
