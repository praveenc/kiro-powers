# Manifest Configuration

Complete guide to package.json configuration for Raycast extensions.

## Extension Properties

```json
{
  "name": "my-extension",
  "title": "My Extension",
  "description": "What my extension does in one sentence",
  "icon": "icon.png",
  "author": "your-raycast-username",
  "platforms": ["macOS"],
  "categories": ["Productivity"],
  "license": "MIT",
  "keywords": ["search", "utility"],
  "commands": [],
  "preferences": []
}
```

### Required Fields

| Field | Description |
|-------|-------------|
| `name` | Unique identifier (URL-safe, lowercase) |
| `title` | Display name in Store and Preferences |
| `description` | One sentence description |
| `icon` | 512x512 PNG in assets folder |
| `author` | Your Raycast username |
| `platforms` | `["macOS"]` or `["macOS", "Windows"]` |
| `categories` | At least one category |
| `commands` | Array of command definitions |

### Categories

```
Applications, Communication, Data, Design Tools, Developer Tools,
Documentation, Finance, Fun, Media, News, Productivity, Security,
System, Web, Other
```

## Command Properties

```json
{
  "commands": [
    {
      "name": "search",
      "title": "Search Items",
      "subtitle": "My Extension",
      "description": "Search through all items",
      "mode": "view",
      "icon": "search-icon.png",
      "keywords": ["find", "lookup"]
    }
  ]
}
```

### Command Modes

| Mode | Description |
|------|-------------|
| `view` | Shows UI (List, Form, Detail, Grid) |
| `no-view` | Runs script, closes Raycast |
| `menu-bar` | Menu bar extra |

### Background Commands

```json
{
  "name": "sync",
  "title": "Sync Data",
  "mode": "no-view",
  "interval": "10m"
}
```

Interval values: `90s`, `1m`, `5m`, `10m`, `15m`, `30m`, `1h`, `6h`, `12h`, `1d`

### Command with Arguments

```json
{
  "name": "search",
  "title": "Search",
  "mode": "view",
  "arguments": [
    {
      "name": "query",
      "type": "text",
      "placeholder": "Search term",
      "required": true
    },
    {
      "name": "category",
      "type": "dropdown",
      "placeholder": "Category",
      "required": false,
      "data": [
        { "title": "All", "value": "all" },
        { "title": "Documents", "value": "docs" }
      ]
    }
  ]
}
```

Access in code:

```tsx
export default function Command(props: { arguments: { query: string; category?: string } }) {
  const { query, category } = props.arguments;
  // ...
}
```

## Preferences

### Extension-Level Preferences

```json
{
  "preferences": [
    {
      "name": "apiKey",
      "title": "API Key",
      "description": "Your API key from the dashboard",
      "type": "password",
      "required": true
    },
    {
      "name": "defaultView",
      "title": "Default View",
      "description": "View to show on launch",
      "type": "dropdown",
      "required": false,
      "default": "list",
      "data": [
        { "title": "List View", "value": "list" },
        { "title": "Grid View", "value": "grid" }
      ]
    }
  ]
}
```

### Command-Level Preferences

```json
{
  "commands": [
    {
      "name": "search",
      "title": "Search",
      "mode": "view",
      "preferences": [
        {
          "name": "showDetails",
          "title": "Show Details",
          "description": "Show detail panel by default",
          "type": "checkbox",
          "label": "Enable detail panel",
          "required": false,
          "default": true
        }
      ]
    }
  ]
}
```

### Preference Types

| Type | Description | Additional Props |
|------|-------------|------------------|
| `textfield` | Single-line text | `placeholder` |
| `password` | Secure text entry | `placeholder` |
| `checkbox` | Boolean toggle | `label` |
| `dropdown` | Select from options | `data` |
| `appPicker` | Select application | - |
| `file` | File picker | - |
| `directory` | Folder picker | - |

### Accessing Preferences

```tsx
import { getPreferenceValues } from "@raycast/api";

interface Preferences {
  apiKey: string;
  defaultView: "list" | "grid";
  showDetails: boolean;
}

export default function Command() {
  const preferences = getPreferenceValues<Preferences>();

  console.log(preferences.apiKey);
  console.log(preferences.defaultView);
}
```

## Tool Properties (AI Extensions)

```json
{
  "tools": [
    {
      "name": "get-items",
      "title": "Get Items",
      "description": "Retrieve all items from the database"
    },
    {
      "name": "create-item",
      "title": "Create Item",
      "description": "Create a new item with title and description",
      "icon": "plus-icon.png"
    }
  ]
}
```

Tool `name` maps to `src/tools/{name}.ts`.

## AI Properties

```json
{
  "ai": {
    "instructions": "When listing items, group by category. Always confirm before deleting."
  }
}
```

Or use separate `ai.json` / `ai.yaml` file for longer instructions and evals.

## Complete Example

```json
{
  "name": "task-manager",
  "title": "Task Manager",
  "description": "Manage your tasks and todos efficiently",
  "icon": "icon.png",
  "author": "johndoe",
  "platforms": ["macOS"],
  "categories": ["Productivity"],
  "license": "MIT",
  "keywords": ["tasks", "todos", "productivity"],
  "preferences": [
    {
      "name": "apiToken",
      "title": "API Token",
      "description": "Your API token from settings",
      "type": "password",
      "required": true
    }
  ],
  "commands": [
    {
      "name": "list-tasks",
      "title": "List Tasks",
      "subtitle": "Task Manager",
      "description": "View all your tasks",
      "mode": "view"
    },
    {
      "name": "create-task",
      "title": "Create Task",
      "description": "Create a new task",
      "mode": "view",
      "arguments": [
        {
          "name": "title",
          "type": "text",
          "placeholder": "Task title",
          "required": false
        }
      ]
    },
    {
      "name": "sync",
      "title": "Sync Tasks",
      "description": "Sync tasks in background",
      "mode": "no-view",
      "interval": "15m",
      "disabledByDefault": true
    }
  ],
  "tools": [
    {
      "name": "get-tasks",
      "title": "Get Tasks",
      "description": "Get all tasks, optionally filtered by status"
    },
    {
      "name": "create-task",
      "title": "Create Task",
      "description": "Create a new task with title and optional due date"
    }
  ],
  "ai": {
    "instructions": "Tasks have title, description, status (todo/done), and optional due date (YYYY-MM-DD)."
  },
  "dependencies": {
    "@raycast/api": "^1.50.0"
  },
  "devDependencies": {
    "@raycast/eslint-config": "^1.0.0",
    "typescript": "^5.0.0"
  },
  "scripts": {
    "build": "ray build",
    "dev": "ray develop",
    "lint": "ray lint",
    "publish": "npx @raycast/api@latest publish"
  }
}
```

## Naming Conventions

- **Extension title**: Title Case, descriptive (`Task Manager`, `GitHub Search`)
- **Command title**: Verb + Noun (`Search Tasks`, `Create Issue`)
- **Subtitle**: Service name for context (`GitHub`, `Notion`)
- **Description**: One clear sentence, no period at end
