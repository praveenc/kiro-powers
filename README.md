# Kiro Powers Collection

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Kiro](https://img.shields.io/badge/Kiro-Powers-blue)](https://kiro.dev)
[![Python 3.13](https://img.shields.io/badge/Python-3.13-3776AB?logo=python&logoColor=white)](https://python.org)
[![React](https://img.shields.io/badge/React-19.2-61DAFB?logo=react&logoColor=black)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5+-3178C6?logo=typescript&logoColor=white)](https://typescriptlang.org)

A curated collection of Kiro Powers for building MCP servers, React applications, and Raycast extensions.

## Available Powers

| Power                                          | Description                    | Keywords                                       |
| ---------------------------------------------- | ------------------------------ | ---------------------------------------------- |
| [MCP Maker](#mcp-maker)                        | Build MCP servers with FastMCP | `mcp server`, `mcp-server`, `fastmcp`, `stdio` |
| [Cloudscape Design](#cloudscape-design-system) | React apps with AWS Cloudscape | `cloudscape`, `react`, `aws-ui`, `components`  |
| [Raycast Extensions](#raycast-extensions)      | Build Raycast extensions       | `raycast`, `extension`, `macos`, `typescript`  |

## Installation

### From GitHub

1. Open Kiro → Powers panel
2. Click **Add power from GitHub**
3. Enter power URL: for e.g. `mcp-maker` power enter `https://github.com/praveenc/kiro-powers/tree/main/mcp-maker`
4. Select the power(s) to install

### From Local Path

```bash
# clone entire repo to disk
git clone https://github.com/praveenc/kiro-powers.git
```

Then in Kiro:

1. Open Powers panel
2. Click **Add power from Local Path**
3. Select the desired power directory from the clone repo

---

## MCP Maker

![MCP](https://img.shields.io/badge/MCP-Protocol-green)
![FastMCP](https://img.shields.io/badge/FastMCP-Framework-orange)
![Python 3.13](https://img.shields.io/badge/Python-3.13-3776AB?logo=python&logoColor=white)

### Overview

MCP Maker Kiro Power helps you build Model Context Protocol (MCP) servers using Python and the FastMCP framework.

#### Features

- **Live documentation lookup** - Search and fetch docs from modelcontextprotocol.io and gofastmcp.com
- **Curated quickstart patterns** - Minimal server templates and best practices
- **Agent-optimized tool design** - Learn how to build tools that AI agents can use effectively

### What's Included

#### MCP Servers

- [mcp-server-builder](https://github.com/praveenc/mcp-server-builder) - MCP server for searching MCP protocol docs and FastMCP framework from their corresponding `llms.txt` files.

#### Steering Files

| File                 | Purpose                                           |
| -------------------- | ------------------------------------------------- |
| `getting-started.md` | Quickstart guide, server templates, configuration |
| `tool-design.md`     | Tool naming, responses, token efficiency          |

### Activating this Kiro Power

Once installed, mention MCP-related keywords in your Kiro chat:

```markdown
"Help me build a mcp-server in python using stdio transport"
"Create a mcp server with a tool for searching contacts"
"How do I design tools for AI agents?"
```

Kiro will automatically activate the MCP Maker power and load relevant context.

### Example Prompt

Copy the below prompt and paste it into Kiro IDE chat window and see a fully working MCP server built for you (one-shotted) in no time. 🎉

```markdown
Build a new `stock-price-watch` mcp-server using stdio transport with the following tools.

- `list_available_tickers` - List all available Free stock tickers to query on financialdatasets.ai API endpoint.
- `compare_stocks` - Compare multiple stocks side by side.
- `get_stock_snapshot` - Get current stock price snapshot with key metrics.

Follow these instructions:

1. Use the financialdatasets.ai API endpoint: <https://api.financialdatasets.ai/prices/snapshot/?ticker={ticker}>
2. Set appropriate headers including User-Agent: "Mozilla/0.1" and Accept: "application/json"
3. Parse the response to extract key metrics like price, volume, day_change, and day_change_percent

When displaying responses:

- Format financial data in a professional, easy-to-read manner
- Highlight important metrics like current price, volume, and daily changes
- Provide context for price movements (positive/negative changes)
- Handle errors appropriately and suggest alternative tickers if needed
- Convert raw data into meaningful financial insights

We'll be using the free endpoint (no API KEY) only a few tickers are accessible.\
Note: Available tickers on free endpoint are: Available free tickers: AAPL, BRK.B, GOOGL, MSFT, NVDA, TSLA.

Use your best judgement.
```

You should have plenty of context left for additional tweaks and updates - even after the requested server fully built.

Here's another. Build **weather-watch** mcp server.

````markdown
Build a new `weather-watch` mcp server in python using stdio transport with the following tools.

- `get_alerts` - Get's weather alerts for a give state.
- `get_lat_long` - Converts US city names to latitude/longitude coordinates.
- `get_forecast` - Get weather forecase for a give latitude, longitude coordinates.

When a User asks: "What's the forecast for San Francisco?" LLM makes the following tool calls.

```bash
1. get_lat_long("San Francisco") → 37.7749, -122.4194
2. get_forecast(37.7749, -122.4194) → Detailed forecast
```

Use these following free API endpoints to query weather data

- GEOCODE_API_BASE = "https://geocode.xyz"
- NWS_API_BASE = "https://api.weather.gov"

Follow these instructions:

1. Ask clarifying questions relevant for building this mcp server.
2. Always get forecast for next 7 days and format the results neatly.
````

---

## Cloudscape Design System

![React](https://img.shields.io/badge/React-19.2-61DAFB?logo=react&logoColor=black)
![AWS](https://img.shields.io/badge/AWS-Cloudscape-FF9900?logo=amazon-aws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-Supported-3178C6?logo=typescript&logoColor=white)

Build professional, accessible React applications using the AWS Cloudscape Design System - the same system powering the AWS Management Console.

### Features

- 90+ production-ready React components with built-in accessibility (WCAG 2.1 AA)
- Token-based theming with light/dark mode and content density settings
- Comprehensive patterns for layouts, forms, tables, charts, and GenAI interfaces
- Full TypeScript support with typed components and event handlers

### Steering Files

| File                       | Purpose                                    |
| -------------------------- | ------------------------------------------ |
| `foundations.md`           | Design tokens, spacing, colors, typography |
| `layout-patterns.md`       | AppLayout, containers, grids               |
| `form-patterns.md`         | Forms, inputs, validation                  |
| `table-and-collections.md` | Tables, cards, filtering, pagination       |
| `navigation-patterns.md`   | Sidebar, tabs, breadcrumbs                 |
| `feedback-patterns.md`     | Alerts, modals, status indicators          |
| `charts-and-data-viz.md`   | Line, bar, pie charts                      |
| `genai-patterns.md`        | Chat bubbles, prompts, AI interfaces       |

### Full Example Prompt

Here's a complete prompt that triggers this Kiro power and builds a functional dashboard:

Copy this prompt and paste it into Kiro IDE to experience a fully working Cloudscape dashboard built for you. 🎉

```markdown
Build a React dashboard application using Cloudscape Design System with the following features:

1. **App Layout**: Use AppLayout with a collapsible side navigation containing:
    - Dashboard (home)
    - Users (list view)
    - Settings

2. **Dashboard Page**: Create a dashboard with:
    - Header showing "Analytics Dashboard" with a refresh button
    - 4 metric cards in a ColumnLayout showing: Total Users, Active Sessions, Revenue, Growth %
    - A LineChart showing user activity over the last 7 days
    - A Table showing recent transactions with columns: ID, User, Amount, Status, Date

3. **Table Features**: The transactions table should include:
    - PropertyFilter for searching by user or status
    - Pagination (10 items per page)
    - Selection checkboxes with bulk actions (Export, Delete)
    - StatusIndicator for transaction status (success=green, pending=yellow, failed=red)

4. **Requirements**:
    - Use TypeScript throughout
    - Support dark mode toggle in the top navigation
    - All components must be accessible (proper ariaLabels)
    - Use design tokens for any custom styling

Use your best judgment for sample data and styling.
```

The power will automatically load relevant steering files for layouts, tables, charts, and navigation. 🎉

### Requirements

- Node.js 18+
- `@cloudscape-design/components`
- `@cloudscape-design/global-styles`

---

## Documentation

- [Kiro Powers Guide](https://kiro.dev/docs/powers/)
- [MCP Protocol](https://modelcontextprotocol.io)
- [FastMCP Framework](https://gofastmcp.com)
- [Cloudscape Design](https://cloudscape.design)
- [Raycast API](https://developers.raycast.com)

## Contributing

For issues or suggestions, please open a GitHub issue.

## License

MIT License - See [LICENSE](LICENSE) for details.

## Author

Praveen Chamarthi
