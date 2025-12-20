# Cloudscape Feedback Patterns

Alerts, notifications, modals, and status indicators.

## Alert

Contextual messages within page content:

```jsx
import { Alert, Button } from '@cloudscape-design/components';

// Types: info (default), success, warning, error
<Alert type="success" header="Resource created">
  Your resource was created successfully.
</Alert>

// With action
<Alert type="warning" header="Limit reached" action={<Button>Upgrade</Button>}>
  You've reached your resource limit.
</Alert>

// Dismissible
const [visible, setVisible] = useState(true);
{visible && (
  <Alert
    type="info"
    dismissible
    dismissAriaLabel="Close"
    onDismiss={() => setVisible(false)}
  >
    New features available.
  </Alert>
)}
```

## Flashbar

Page-level notifications (use in AppLayout.notifications):

```jsx
import { Flashbar } from '@cloudscape-design/components';

const [notifications, setNotifications] = useState([]);

const addNotification = (type, header, content) => {
  const id = Date.now().toString();
  setNotifications(prev => [...prev, {
    id,
    type,
    header,
    content,
    dismissible: true,
    onDismiss: () => setNotifications(prev => prev.filter(n => n.id !== id)),
  }]);
};

<AppLayout
  notifications={<Flashbar items={notifications} />}
  content={...}
/>

// Add notifications
addNotification('success', 'Created', 'Resource created successfully.');
addNotification('error', 'Failed', 'Could not delete resource.');
```

### Flashbar with Loading

```jsx
const [items, setItems] = useState([{
  id: 'loading',
  type: 'info',
  loading: true,
  header: 'Creating resource',
  content: 'Please wait...',
  dismissible: false,
}]);

// On completion, replace with success/error
setItems([{
  id: 'success',
  type: 'success',
  header: 'Resource created',
  dismissible: true,
  onDismiss: () => setItems([]),
}]);
```

## StatusIndicator

Resource state display:

```jsx
import { StatusIndicator } from '@cloudscape-design/components';

<StatusIndicator type="success">Running</StatusIndicator>
<StatusIndicator type="error">Failed</StatusIndicator>
<StatusIndicator type="warning">At risk</StatusIndicator>
<StatusIndicator type="info">In progress</StatusIndicator>
<StatusIndicator type="pending">Pending</StatusIndicator>
<StatusIndicator type="stopped">Stopped</StatusIndicator>
<StatusIndicator type="loading">Loading</StatusIndicator>
```

## Modal

Focused interactions requiring user attention:

```jsx
import { Modal, Box, SpaceBetween, Button } from '@cloudscape-design/components';

const [visible, setVisible] = useState(false);

<Button onClick={() => setVisible(true)}>Delete</Button>

<Modal
  visible={visible}
  onDismiss={() => setVisible(false)}
  header="Delete resource"
  footer={
    <Box float="right">
      <SpaceBetween direction="horizontal" size="xs">
        <Button variant="link" onClick={() => setVisible(false)}>Cancel</Button>
        <Button variant="primary" onClick={handleDelete}>Delete</Button>
      </SpaceBetween>
    </Box>
  }
>
  Are you sure you want to delete this resource? This action cannot be undone.
</Modal>
```

### Modal Sizes

```jsx
<Modal size="small">...</Modal>   // 320px
<Modal size="medium">...</Modal>  // 600px (default)
<Modal size="large">...</Modal>   // 820px
<Modal size="max">...</Modal>     // Full viewport
```

## ProgressBar

For operations with known duration:

```jsx
import { ProgressBar } from '@cloudscape-design/components';

<ProgressBar
  value={progress}
  label="Uploading file"
  description="file.txt"
  additionalInfo={`${progress}% complete`}
/>

// With status
<ProgressBar
  value={100}
  status="success"
  resultText="Upload complete"
/>

<ProgressBar
  value={45}
  status="error"
  resultText="Upload failed"
/>
```

## Spinner

For loading states:

```jsx
import { Spinner, Box } from '@cloudscape-design/components';

// Inline
<Spinner /> Loading...

// Centered
<Box textAlign="center" padding="l">
  <Spinner size="large" />
</Box>

// Sizes: normal, big, large
<Spinner size="large" />
```

## Popover

Contextual information on hover/click:

```jsx
import { Popover, Button, StatusIndicator } from '@cloudscape-design/components';

<Popover
  header="Instance status"
  content="The instance is currently running and healthy."
  triggerType="custom"
>
  <StatusIndicator type="success">Running</StatusIndicator>
</Popover>

// With dismiss button
<Popover
  dismissButton={true}
  header="Help"
  content="Click here to learn more about this feature."
>
  <Button iconName="status-info" variant="icon" />
</Popover>
```

## Badge

Small visual labels:

```jsx
import { Badge } from '@cloudscape-design/components';

<Badge>Default</Badge>
<Badge color="blue">New</Badge>
<Badge color="red">Critical</Badge>
<Badge color="green">Active</Badge>
<Badge color="grey">Archived</Badge>
```

## HelpPanel

Contextual help in tools panel:

```jsx
import { HelpPanel, Link } from '@cloudscape-design/components';

<AppLayout
  tools={
    <HelpPanel
      header={<h2>Resource configuration</h2>}
      footer={
        <div>
          <h3>Learn more</h3>
          <Link external href="https://docs.example.com">Documentation</Link>
        </div>
      }
    >
      <p>Configure your resource settings here.</p>
      <h3>Instance type</h3>
      <p>Choose the instance type based on your workload requirements.</p>
    </HelpPanel>
  }
/>
```

## Feedback Patterns Summary

| Component | Use Case | Duration |
|-----------|----------|----------|
| Alert | Contextual, inline messages | Persistent |
| Flashbar | Page-level notifications | Temporary (dismissible) |
| StatusIndicator | Resource state | Persistent |
| Modal | Focused interaction, confirmation | Until dismissed |
| Spinner | Loading (1-10 seconds) | Until complete |
| ProgressBar | Long operations (10+ seconds) | Until complete |
| Popover | Contextual help/info | On hover/click |

## Best Practices

1. Use Alert for contextual messages near related content
2. Use Flashbar for page-level action results
3. Make success messages dismissible, auto-dismiss after 5-10s
4. Keep error messages visible until user dismisses
5. Use StatusIndicator for resource states in tables/cards
6. Use Modal for destructive actions requiring confirmation
7. Provide clear action buttons in modals (Cancel + Primary action)
8. Use Spinner for short waits, ProgressBar for longer operations
9. Always include loading text with spinners for accessibility
