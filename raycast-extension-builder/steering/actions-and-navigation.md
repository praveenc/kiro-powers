# Actions and Navigation

User interactions, keyboard shortcuts, and navigation between views.

## ActionPanel Basics

Every interactive component needs an ActionPanel:

```tsx
import { List, ActionPanel, Action, Icon } from "@raycast/api";

export default function Command() {
  return (
    <List>
      <List.Item
        title="Item"
        actions={
          <ActionPanel>
            <Action title="Primary Action" onAction={() => console.log("clicked")} />
            <Action.OpenInBrowser url="https://raycast.com" />
          </ActionPanel>
        }
      />
    </List>
  );
}
```

## Built-in Actions

### Common Actions

```tsx
// Open URL in browser
<Action.OpenInBrowser url="https://raycast.com" />

// Copy to clipboard
<Action.CopyToClipboard content="Hello World" />

// Paste to frontmost app
<Action.Paste content="Pasted text" />

// Open file/folder
<Action.Open target="/path/to/file" />
<Action.ShowInFinder path="/path/to/folder" />

// Submit form
<Action.SubmitForm title="Submit" onSubmit={handleSubmit} />

// Navigate to another view
<Action.Push title="Details" target={<DetailView />} />

// Open extension preferences
<Action.OpenExtensionPreferences />
```

### Action with Icon and Shortcut

```tsx
<Action
  title="Delete"
  icon={Icon.Trash}
  style={Action.Style.Destructive}
  shortcut={{ modifiers: ["cmd"], key: "d" }}
  onAction={() => handleDelete()}
/>
```

## Action Sections

Organize actions into logical groups:

```tsx
<ActionPanel>
  <ActionPanel.Section title="Primary">
    <Action title="Open" icon={Icon.Globe} onAction={handleOpen} />
    <Action.CopyToClipboard content={url} />
  </ActionPanel.Section>

  <ActionPanel.Section title="Edit">
    <Action.Push title="Edit" icon={Icon.Pencil} target={<EditForm />} />
    <Action
      title="Delete"
      icon={Icon.Trash}
      style={Action.Style.Destructive}
      onAction={handleDelete}
    />
  </ActionPanel.Section>
</ActionPanel>
```

## Keyboard Shortcuts

### Common Shortcuts

| Shortcut | Convention |
|----------|------------|
| `⌘ + N` | Create new |
| `⌘ + E` | Edit |
| `⌘ + D` | Delete |
| `⌘ + C` | Copy |
| `⌘ + ⇧ + C` | Copy alternate |
| `⌘ + O` | Open |
| `⌘ + R` | Refresh |
| `⌃ + X` | Destructive action |

### Assigning Shortcuts

```tsx
<Action
  title="Create New"
  shortcut={{ modifiers: ["cmd"], key: "n" }}
  onAction={handleCreate}
/>

<Action
  title="Delete"
  shortcut={{ modifiers: ["ctrl"], key: "x" }}
  style={Action.Style.Destructive}
  onAction={handleDelete}
/>
```


## Navigation

### Push/Pop Navigation

```tsx
import { List, ActionPanel, Action, useNavigation, Detail } from "@raycast/api";

function ItemDetail({ item }: { item: string }) {
  return <Detail markdown={`# ${item}\n\nDetails about ${item}`} />;
}

export default function Command() {
  const items = ["Apple", "Banana", "Cherry"];

  return (
    <List>
      {items.map(item => (
        <List.Item
          key={item}
          title={item}
          actions={
            <ActionPanel>
              <Action.Push
                title="View Details"
                target={<ItemDetail item={item} />}
              />
            </ActionPanel>
          }
        />
      ))}
    </List>
  );
}
```

### useNavigation Hook

```tsx
import { useNavigation, Form, Action, ActionPanel, showToast } from "@raycast/api";

function CreateForm({ onCreate }: { onCreate: (name: string) => void }) {
  const { pop } = useNavigation();

  async function handleSubmit(values: { name: string }) {
    onCreate(values.name);
    await showToast({ title: "Created!" });
    pop(); // Go back to previous view
  }

  return (
    <Form
      actions={
        <ActionPanel>
          <Action.SubmitForm title="Create" onSubmit={handleSubmit} />
        </ActionPanel>
      }
    >
      <Form.TextField id="name" title="Name" />
    </Form>
  );
}
```

### Navigation with Callbacks

```tsx
<Action.Push
  title="Create"
  target={<CreateForm />}
  onPush={() => console.log("Navigated to form")}
  onPop={() => {
    // Refresh data when returning
    loadItems();
  }}
/>
```

## Submenu Actions

```tsx
<ActionPanel>
  <ActionPanel.Submenu title="Set Priority" icon={Icon.Flag}>
    <Action title="High" onAction={() => setPriority("high")} />
    <Action title="Medium" onAction={() => setPriority("medium")} />
    <Action title="Low" onAction={() => setPriority("low")} />
  </ActionPanel.Submenu>
</ActionPanel>
```

## Confirmation Dialogs

```tsx
import { confirmAlert, Alert, Action, ActionPanel } from "@raycast/api";

async function handleDelete(id: string) {
  const confirmed = await confirmAlert({
    title: "Delete Item?",
    message: "This action cannot be undone.",
    primaryAction: {
      title: "Delete",
      style: Alert.ActionStyle.Destructive,
    },
  });

  if (confirmed) {
    await deleteItem(id);
  }
}

// In ActionPanel
<Action
  title="Delete"
  icon={Icon.Trash}
  style={Action.Style.Destructive}
  onAction={() => handleDelete(item.id)}
/>
```

## Toast Feedback

```tsx
import { showToast, Toast } from "@raycast/api";

// Simple toast
await showToast({ title: "Success!" });

// Toast with style
await showToast({
  style: Toast.Style.Success,
  title: "Item Created",
  message: "Your item was created successfully",
});

// Animated toast for async operations
const toast = await showToast({
  style: Toast.Style.Animated,
  title: "Loading...",
});

try {
  await fetchData();
  toast.style = Toast.Style.Success;
  toast.title = "Loaded!";
} catch (error) {
  toast.style = Toast.Style.Failure;
  toast.title = "Failed";
  toast.message = String(error);
}
```

## No-View Commands

For commands that don't show UI:

```tsx
import { showToast, Toast, closeMainWindow, Clipboard } from "@raycast/api";

export default async function Command() {
  await closeMainWindow(); // Close Raycast immediately

  const result = await doSomething();
  await Clipboard.copy(result);

  await showToast({
    style: Toast.Style.Success,
    title: "Copied!",
  });
}
```

## Best Practices

1. First action is the default (triggered by `↵`)
2. Use sections to group related actions
3. Add shortcuts to frequently-used actions
4. Use `Action.Style.Destructive` for delete actions
5. Show confirmation for irreversible actions
6. Use `onPop` callback to refresh data after navigation
7. Close main window for quick utility commands
