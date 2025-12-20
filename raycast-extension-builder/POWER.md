---
name: "raycast-extension-builder"
displayName: "Raycast Extensions"
description: "Build, test, and publish Raycast extensions with React, TypeScript, and the Raycast API"
author: "Praveen Chamarthi"
keywords: ["raycast", "extension", "command", "action", "actionpanel", "list", "form", "detail", "grid", "toast", "preferences", "ai-extension", "tool", "evals", "store", "publish"]
---

# Raycast Extensions Power

Build professional Raycast extensions using React, TypeScript, and the Raycast API.

## Onboarding

### Step 1: Validate Environment

Ensure you have the required tools:

- **Node.js** (v16+): `node --version`
- **npm**: `npm --version`
- **Raycast**: Must be installed on macOS

### Step 2: Create Your First Extension

1. Open Raycast and run "Create Extension" command
2. Choose a template (List, Form, Detail, or No View)
3. Select a location for your extension
4. Run in terminal:

```bash
cd your-extension
npm install && npm run dev
```

### Step 3: Development Workflow

- `npm run dev` - Start development mode with hot reload
- `npm run build` - Validate for distribution
- `npm run lint` - Check code style
- `npm run publish` - Publish to store

## Core Patterns

### File Structure

```bash
extension/
├── package.json      # Manifest with commands, preferences
├── src/
│   ├── index.tsx     # Main command entry point
│   └── tools/        # AI tools (if AI extension)
├── assets/
│   └── icon.png      # 512x512 extension icon
└── tsconfig.json
```

### Command Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| `view` | Shows UI (List, Form, Detail, Grid) | Most commands |
| `no-view` | Runs script, closes Raycast | Quick actions, scripts |
| `menu-bar` | Menu bar extra | Background status |

### Essential Imports

```tsx
import {
  List,
  Form,
  Detail,
  Grid,
  Action,
  ActionPanel,
  Icon,
  showToast,
  Toast,
  useNavigation,
  LocalStorage,
  getPreferenceValues,
  Clipboard,
  closeMainWindow
} from "@raycast/api";
```

### Manifest Basics (package.json)

```json
{
  "name": "my-extension",
  "title": "My Extension",
  "description": "What my extension does",
  "icon": "icon.png",
  "author": "your-raycast-username",
  "platforms": ["macOS"],
  "categories": ["Productivity"],
  "license": "MIT",
  "commands": [
    {
      "name": "index",
      "title": "My Command",
      "description": "What this command does",
      "mode": "view"
    }
  ]
}
```

### Component Quick Reference

| Category | Components |
|----------|------------|
| Views | List, Form, Detail, Grid |
| Actions | ActionPanel, Action, Action.OpenInBrowser, Action.CopyToClipboard, Action.Push |
| Feedback | Toast, Alert, confirmAlert |
| Data | LocalStorage, Preferences |
| Navigation | useNavigation, Action.Push |
| Utilities | Clipboard, closeMainWindow, open |

### Event Handling Pattern

Raycast uses consistent event patterns:

```tsx
// Form submission
<Action.SubmitForm onSubmit={(values) => handleSubmit(values)} />

// List selection
<List onSelectionChange={(id) => setSelectedId(id)} />

// Input change
<Form.TextField onChange={(e) => setValue(e.target.value)} />
```

## When to Load Steering Files

- Creating first extension, project setup → `getting-started.md`
- Building commands with List, Form, Detail, Grid → `commands-and-ui.md`
- Adding actions, keyboard shortcuts, navigation → `actions-and-navigation.md`
- Creating AI extensions with tools → `ai-extensions.md`
- Configuring package.json, preferences, arguments → `manifest-config.md`
- Working with LocalStorage, API calls, data fetching → `data-and-storage.md`
- Preparing for store, publishing, review process → `publishing.md`
