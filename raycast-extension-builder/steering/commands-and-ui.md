# Raycast Commands and UI Components

Building user interfaces with List, Form, Detail, and Grid components.

## List Component

The most common component for displaying searchable items.

### Basic List

```tsx
import { List } from "@raycast/api";

export default function Command() {
  return (
    <List>
      <List.Item title="Item 1" subtitle="Description" />
      <List.Item title="Item 2" icon="🎉" />
    </List>
  );
}
```

### List with Loading State

```tsx
import { List } from "@raycast/api";
import { useState, useEffect } from "react";

export default function Command() {
  const [items, setItems] = useState<string[]>([]);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    fetchData().then(data => {
      setItems(data);
      setIsLoading(false);
    });
  }, []);

  return (
    <List isLoading={isLoading}>
      {items.map((item, index) => (
        <List.Item key={index} title={item} />
      ))}
    </List>
  );
}
```

### List with Sections

```tsx
<List>
  <List.Section title="Recent">
    <List.Item title="Project A" />
    <List.Item title="Project B" />
  </List.Section>
  <List.Section title="Favorites">
    <List.Item title="Project C" />
  </List.Section>
</List>
```

### List with Accessories

```tsx
<List.Item
  title="Task"
  subtitle="Project X"
  accessories={[
    { text: "High", icon: Icon.ExclamationMark },
    { date: new Date() },
    { tag: { value: "Bug", color: Color.Red } }
  ]}
/>
```


### List with Detail Panel

```tsx
<List isShowingDetail>
  <List.Item
    title="Item"
    detail={
      <List.Item.Detail
        markdown="# Details\n\nMarkdown content here"
        metadata={
          <List.Item.Detail.Metadata>
            <List.Item.Detail.Metadata.Label title="Status" text="Active" />
            <List.Item.Detail.Metadata.Separator />
            <List.Item.Detail.Metadata.Link
              title="Docs"
              target="https://raycast.com"
              text="Open"
            />
          </List.Item.Detail.Metadata>
        }
      />
    }
  />
</List>
```

### List with Search

```tsx
export default function Command() {
  const [searchText, setSearchText] = useState("");
  const [items] = useState(["Apple", "Banana", "Cherry"]);

  const filtered = items.filter(item =>
    item.toLowerCase().includes(searchText.toLowerCase())
  );

  return (
    <List
      searchBarPlaceholder="Search fruits..."
      onSearchTextChange={setSearchText}
    >
      {filtered.map(item => (
        <List.Item key={item} title={item} />
      ))}
    </List>
  );
}
```

### List with Dropdown Filter

```tsx
<List
  searchBarAccessory={
    <List.Dropdown tooltip="Filter" onChange={setFilter}>
      <List.Dropdown.Item title="All" value="all" />
      <List.Dropdown.Item title="Active" value="active" />
      <List.Dropdown.Item title="Completed" value="completed" />
    </List.Dropdown>
  }
>
  {/* items */}
</List>
```

### Empty State

```tsx
<List>
  <List.EmptyView
    title="No Results"
    description="Try a different search term"
    icon="🔍"
    actions={
      <ActionPanel>
        <Action title="Create New" onAction={() => {}} />
      </ActionPanel>
    }
  />
</List>
```

## Form Component

For collecting user input.

### Basic Form

```tsx
import { Form, Action, ActionPanel, showToast } from "@raycast/api";

interface FormValues {
  name: string;
  email: string;
  role: string;
}

export default function Command() {
  function handleSubmit(values: FormValues) {
    showToast({ title: "Submitted", message: values.name });
  }

  return (
    <Form
      actions={
        <ActionPanel>
          <Action.SubmitForm title="Submit" onSubmit={handleSubmit} />
        </ActionPanel>
      }
    >
      <Form.TextField id="name" title="Name" placeholder="Enter name" />
      <Form.TextField id="email" title="Email" placeholder="Enter email" />
      <Form.Dropdown id="role" title="Role">
        <Form.Dropdown.Item value="user" title="User" />
        <Form.Dropdown.Item value="admin" title="Admin" />
      </Form.Dropdown>
      <Form.Checkbox id="subscribe" label="Subscribe to newsletter" />
    </Form>
  );
}
```


### Form Field Types

```tsx
// Text inputs
<Form.TextField id="name" title="Name" />
<Form.TextArea id="description" title="Description" enableMarkdown />
<Form.PasswordField id="token" title="API Token" />

// Selection
<Form.Dropdown id="type" title="Type">
  <Form.Dropdown.Item value="a" title="Option A" />
</Form.Dropdown>

<Form.TagPicker id="tags" title="Tags">
  <Form.TagPicker.Item value="bug" title="Bug" icon="🐛" />
  <Form.TagPicker.Item value="feature" title="Feature" icon="✨" />
</Form.TagPicker>

// Boolean
<Form.Checkbox id="enabled" label="Enable feature" />

// Date/File
<Form.DatePicker id="date" title="Due Date" />
<Form.FilePicker id="file" title="Upload" />
```

### Form with Validation

```tsx
import { Form, Action, ActionPanel, showToast, Toast } from "@raycast/api";
import { useState } from "react";

export default function Command() {
  const [nameError, setNameError] = useState<string | undefined>();
  const [emailError, setEmailError] = useState<string | undefined>();

  function validateName(value: string | undefined) {
    if (!value || value.length < 2) {
      setNameError("Name must be at least 2 characters");
      return false;
    }
    setNameError(undefined);
    return true;
  }

  function validateEmail(value: string | undefined) {
    if (!value || !/\S+@\S+\.\S+/.test(value)) {
      setEmailError("Invalid email address");
      return false;
    }
    setEmailError(undefined);
    return true;
  }

  async function handleSubmit(values: { name: string; email: string }) {
    const isNameValid = validateName(values.name);
    const isEmailValid = validateEmail(values.email);

    if (!isNameValid || !isEmailValid) {
      await showToast({ style: Toast.Style.Failure, title: "Validation Error" });
      return;
    }

    await showToast({ style: Toast.Style.Success, title: "Submitted!" });
  }

  return (
    <Form
      actions={
        <ActionPanel>
          <Action.SubmitForm title="Submit" onSubmit={handleSubmit} />
        </ActionPanel>
      }
    >
      <Form.TextField
        id="name"
        title="Name"
        error={nameError}
        onChange={(value) => validateName(value)}
      />
      <Form.TextField
        id="email"
        title="Email"
        error={emailError}
        onChange={(value) => validateEmail(value)}
      />
    </Form>
  );
}
```

### Form with Stored Values

```tsx
<Form.Dropdown id="expiry" title="Expiry" storeValue>
  <Form.Dropdown.Item value="1" title="1 Day" />
  <Form.Dropdown.Item value="7" title="1 Week" />
</Form.Dropdown>
```

The `storeValue` prop remembers the last selected value.

## Detail Component

For displaying rich markdown content.

```tsx
import { Detail, ActionPanel, Action } from "@raycast/api";

export default function Command() {
  const markdown = `
# Project Overview

This is **bold** and *italic* text.

## Features
- Feature 1
- Feature 2

\`\`\`typescript
const hello = "world";
\`\`\`
  `;

  return (
    <Detail
      markdown={markdown}
      actions={
        <ActionPanel>
          <Action.CopyToClipboard content={markdown} />
        </ActionPanel>
      }
      metadata={
        <Detail.Metadata>
          <Detail.Metadata.Label title="Version" text="1.0.0" />
          <Detail.Metadata.Link title="Docs" target="https://raycast.com" text="Open" />
          <Detail.Metadata.Separator />
          <Detail.Metadata.TagList title="Tags">
            <Detail.Metadata.TagList.Item text="React" color="#61dafb" />
            <Detail.Metadata.TagList.Item text="TypeScript" color="#3178c6" />
          </Detail.Metadata.TagList>
        </Detail.Metadata>
      }
    />
  );
}
```

## Grid Component

For visual content like images or colors.

```tsx
import { Grid } from "@raycast/api";

export default function Command() {
  return (
    <Grid columns={4} inset={Grid.Inset.Medium}>
      <Grid.Item
        content="https://example.com/image.png"
        title="Image 1"
        subtitle="Description"
      />
      <Grid.Item
        content={{ color: { light: "#ff0000", dark: "#ff6666" } }}
        title="Red"
      />
      <Grid.Item content={Icon.Star} title="Icon" />
    </Grid>
  );
}
```

### Grid with Sections

```tsx
<Grid>
  <Grid.Section title="Recent">
    <Grid.Item content="image1.png" title="Item 1" />
  </Grid.Section>
  <Grid.Section title="Favorites">
    <Grid.Item content="image2.png" title="Item 2" />
  </Grid.Section>
</Grid>
```

## Best Practices

1. Always show `isLoading={true}` while fetching data
2. Provide meaningful empty states with actions
3. Use sections to organize long lists
4. Keep forms minimal - only required fields
5. Use `storeValue` for frequently-used dropdowns
6. Add keyboard shortcuts to common actions
