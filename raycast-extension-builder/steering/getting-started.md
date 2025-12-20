# Getting Started with Raycast Extensions

First-time setup and creating your first extension.

## Prerequisites

- **Node.js** v16 or higher
- **npm** (comes with Node.js)
- **Raycast** app installed on macOS

## Create Extension

### Using Raycast Command (Recommended)

1. Open Raycast
2. Search for "Create Extension"
3. Fill in:
   - Extension name
   - Template (List, Form, Detail, No View)
   - Location on disk
4. Press `⌘ + ↵` to create

### Manual Setup

```bash
mkdir my-extension && cd my-extension
npm init -y
npm install @raycast/api
```

## File Structure

```
my-extension/
├── package.json        # Manifest + dependencies
├── src/
│   └── index.tsx       # Main command
├── assets/
│   └── icon.png        # 512x512 extension icon
├── tsconfig.json       # TypeScript config
├── eslint.config.js    # Linting rules
└── .prettierrc         # Code formatting
```

## Development Mode

```bash
cd my-extension
npm install
npm run dev
```

This starts hot-reloading. Your extension appears in Raycast's root search under "Development".

## Minimal List Command

```tsx
// src/index.tsx
import { List } from "@raycast/api";

export default function Command() {
  return (
    <List>
      <List.Item title="Hello World" />
      <List.Item title="Welcome to Raycast" />
    </List>
  );
}
```


## Minimal Form Command

```tsx
// src/create-item.tsx
import { Form, Action, ActionPanel, showToast, Toast } from "@raycast/api";

export default function Command() {
  async function handleSubmit(values: { name: string }) {
    await showToast({ title: "Created", message: values.name });
  }

  return (
    <Form
      actions={
        <ActionPanel>
          <Action.SubmitForm title="Create" onSubmit={handleSubmit} />
        </ActionPanel>
      }
    >
      <Form.TextField id="name" title="Name" placeholder="Enter name" />
    </Form>
  );
}
```

## Minimal Detail Command

```tsx
// src/show-info.tsx
import { Detail } from "@raycast/api";

export default function Command() {
  return <Detail markdown="# Hello\n\nThis is **markdown** content." />;
}
```

## Minimal No-View Command

```tsx
// src/quick-action.tsx
import { showToast, Toast, closeMainWindow } from "@raycast/api";

export default async function Command() {
  await closeMainWindow();
  await showToast({ style: Toast.Style.Success, title: "Done!" });
}
```

## Debugging

### Console Logging

```tsx
console.log("Debug message");
console.error("Error message");
```

Logs appear in terminal running `npm run dev`.

### Error Overlay

Unhandled errors show an overlay in development mode with stack trace and "Jump to Error" action.

### React DevTools

```bash
npm install --save-dev react-devtools@6.1.1
```

Then press `⌘ + ⌥ + D` in Raycast to open DevTools.

## Common Issues

### Extension Not Showing

- Ensure `npm run dev` is running
- Check for TypeScript errors in terminal
- Verify `package.json` has valid `commands` array

### Hot Reload Not Working

- Save the file
- Check terminal for compilation errors
- Restart `npm run dev` if needed

### Icon Not Displaying

- Must be 512x512 PNG
- Place in `assets/` folder
- Reference as `"icon": "icon.png"` in package.json

## Next Steps

1. Add more commands to `package.json`
2. Implement actions (see `actions-and-navigation.md`)
3. Add preferences for configuration
4. Prepare for publishing (see `publishing.md`)
