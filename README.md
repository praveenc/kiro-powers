# Kiro Powers Collection

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Kiro](https://img.shields.io/badge/Kiro-Powers-blue)](https://kiro.dev)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://python.org)
[![React](https://img.shields.io/badge/React-18+-61DAFB?logo=react&logoColor=black)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5+-3178C6?logo=typescript&logoColor=white)](https://typescriptlang.org)

A curated collection of Kiro Powers for building MCP servers, React applications, and Raycast extensions.

## Available Powers

| Power | Description | Keywords |
|-------|-------------|----------|
| [MCP Maker](#mcp-maker) | Build MCP servers with FastMCP | `mcp`, `fastmcp`, `python`, `tools` |
| [Cloudscape Design](#cloudscape-design-system) | React apps with AWS Cloudscape | `cloudscape`, `react`, `aws-ui`, `components` |
| [Raycast Extensions](#raycast-extensions) | Build Raycast extensions | `raycast`, `extension`, `macos`, `typescript` |

## Installation

### From GitHub

1. Open Kiro → Powers panel
2. Click **Add power from GitHub**
3. Enter: `https://github.com/praveenc/kiro-powers`
4. Select the power(s) to install

### From Local Path

```bash
git clone https://github.com/praveenc/kiro-powers.git
```

Then in Kiro:

1. Open Powers panel
2. Click **Add power from Local Path**
3. Select the desired power directory

---

## MCP Maker

![MCP](https://img.shields.io/badge/MCP-Protocol-green)
![FastMCP](https://img.shields.io/badge/FastMCP-Framework-orange)

Build production-ready MCP servers in Python with FastMCP framework, live docs, and agent-optimized tool design patterns.

### Features

- Live documentation lookup from modelcontextprotocol.io and gofastmcp.com
- Curated quickstart patterns and minimal server templates
- Agent-optimized tool design best practices

### Steering Files

| File | Purpose |
|------|---------|
| `getting-started.md` | Quickstart guide, server templates, configuration |
| `tool-design.md` | Tool naming, responses, token efficiency |

### Quick Example

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("my-server")

@mcp.tool()
def search_contacts(query: str, limit: int = 10) -> list[dict]:
    """Search contacts by name, email, or company."""
    return results

if __name__ == "__main__":
    mcp.run(transport='stdio')
```

### Requirements

- Python 3.10+
- `uv` package manager

---

## Cloudscape Design System

![React](https://img.shields.io/badge/React-18+-61DAFB?logo=react&logoColor=black)
![AWS](https://img.shields.io/badge/AWS-Cloudscape-FF9900?logo=amazon-aws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-Supported-3178C6?logo=typescript&logoColor=white)

Build professional, accessible React applications using the AWS Cloudscape Design System - the same system powering the AWS Management Console.

### Features

- 90+ production-ready React components with built-in accessibility (WCAG 2.1 AA)
- Token-based theming with light/dark mode and content density settings
- Comprehensive patterns for layouts, forms, tables, charts, and GenAI interfaces
- Full TypeScript support with typed components and event handlers

### Steering Files

| File | Purpose |
|------|---------|
| `foundations.md` | Design tokens, spacing, colors, typography |
| `layout-patterns.md` | AppLayout, containers, grids |
| `form-patterns.md` | Forms, inputs, validation |
| `table-and-collections.md` | Tables, cards, filtering, pagination |
| `navigation-patterns.md` | Sidebar, tabs, breadcrumbs |
| `feedback-patterns.md` | Alerts, modals, status indicators |
| `charts-and-data-viz.md` | Line, bar, pie charts |
| `genai-patterns.md` | Chat bubbles, prompts, AI interfaces |

### Quick Example

```tsx
import '@cloudscape-design/global-styles/index.css';
import { Button, Container, Header } from '@cloudscape-design/components';

function App() {
  return (
    <Container header={<Header variant="h1">My App</Header>}>
      <Button variant="primary">Get Started</Button>
    </Container>
  );
}
```

### Full Example Prompt

Here's a complete prompt that triggers this Kiro power and builds a functional dashboard:

```text
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

Copy this prompt and paste it into Kiro IDE to experience a fully working Cloudscape dashboard built for you. The power will automatically load relevant steering files for layouts, tables, charts, and navigation. 🎉

### Requirements

- Node.js 18+
- `@cloudscape-design/components`
- `@cloudscape-design/global-styles`

---

## Raycast Extensions

![Raycast](https://img.shields.io/badge/Raycast-Extension-FF6363)
![macOS](https://img.shields.io/badge/macOS-Only-000000?logo=apple&logoColor=white)

Build, test, and publish Raycast extensions with React, TypeScript, and the Raycast API.

### Features

- Full Raycast API support (List, Form, Detail, Grid)
- AI extension patterns with tools and evals
- Store publishing workflow

### Steering Files

| File | Purpose |
|------|---------|
| `getting-started.md` | Project setup, first extension |
| `commands-and-ui.md` | List, Form, Detail, Grid components |
| `actions-and-navigation.md` | ActionPanel, shortcuts, navigation |
| `ai-extensions.md` | AI tools, evals, prompts |
| `manifest-config.md` | package.json, preferences, arguments |
| `data-and-storage.md` | LocalStorage, API calls, caching |
| `publishing.md` | Store submission, review process |

### Quick Example

```tsx
import { List, ActionPanel, Action } from "@raycast/api";

export default function Command() {
  return (
    <List>
      <List.Item
        title="Hello World"
        actions={
          <ActionPanel>
            <Action.CopyToClipboard content="Hello!" />
          </ActionPanel>
        }
      />
    </List>
  );
}
```

### Requirements

- Node.js 16+
- Raycast (macOS)

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
