# Cloudscape Navigation Patterns

Side navigation, tabs, breadcrumbs, and buttons.

## SideNavigation

```jsx
import { SideNavigation } from '@cloudscape-design/components';

<SideNavigation
  activeHref="#/dashboard"
  header={{ text: 'My App', href: '#/' }}
  items={[
    { type: 'link', text: 'Dashboard', href: '#/dashboard' },
    { type: 'section', text: 'Resources', items: [
      { type: 'link', text: 'Instances', href: '#/instances' },
      { type: 'link', text: 'Storage', href: '#/storage' },
    ]},
    { type: 'divider' },
    { type: 'link', text: 'Settings', href: '#/settings' },
  ]}
  onFollow={event => {
    event.preventDefault();
    // Custom navigation (e.g., router.push(event.detail.href))
  }}
/>
```

### Item Types

| Type | Use |
|------|-----|
| `link` | Simple navigation link |
| `section` | Collapsible group of links |
| `expandable-link` | Link that expands to show children |
| `divider` | Horizontal separator |
| `section-group` | Non-collapsible group of sections |

## BreadcrumbGroup

```jsx
import { BreadcrumbGroup } from '@cloudscape-design/components';

<BreadcrumbGroup
  items={[
    { text: 'Home', href: '/' },
    { text: 'Resources', href: '/resources' },
    { text: 'Instance i-1234', href: '/resources/i-1234' },
  ]}
  onFollow={event => {
    event.preventDefault();
    // Custom navigation
  }}
/>
```

## Tabs

```jsx
import { Tabs } from '@cloudscape-design/components';

const [activeTab, setActiveTab] = useState('overview');

<Tabs
  activeTabId={activeTab}
  onChange={({ detail }) => setActiveTab(detail.activeTabId)}
  tabs={[
    { id: 'overview', label: 'Overview', content: <OverviewPanel /> },
    { id: 'details', label: 'Details', content: <DetailsPanel /> },
    { id: 'monitoring', label: 'Monitoring', content: <MonitoringPanel /> },
    { id: 'tags', label: 'Tags', disabled: true },
  ]}
/>
```

## Button

### Variants

```jsx
// Primary - main action (one per section)
<Button variant="primary">Create resource</Button>

// Normal - secondary actions
<Button variant="normal">Edit</Button>

// Link - tertiary/cancel actions
<Button variant="link">Cancel</Button>

// Icon button
<Button iconName="refresh" iconAlign="icon-only" ariaLabel="Refresh" />

// With icon
<Button iconName="add-plus">Add item</Button>
<Button iconName="external" iconAlign="right">Documentation</Button>
```

### Loading State

```jsx
<Button
  variant="primary"
  loading={isLoading}
  loadingText="Saving..."
  onClick={handleSave}
>
  Save changes
</Button>
```

### Button Groups

```jsx
<SpaceBetween direction="horizontal" size="xs">
  <Button variant="link">Cancel</Button>
  <Button variant="normal">Reset</Button>
  <Button variant="primary">Submit</Button>
</SpaceBetween>
```

## ButtonDropdown

```jsx
import { ButtonDropdown } from '@cloudscape-design/components';

<ButtonDropdown
  items={[
    { id: 'edit', text: 'Edit' },
    { id: 'duplicate', text: 'Duplicate' },
    { id: 'delete', text: 'Delete', disabled: true },
  ]}
  onItemClick={({ detail }) => handleAction(detail.id)}
>
  Actions
</ButtonDropdown>

// With nested items
<ButtonDropdown
  items={[
    { id: 'export', text: 'Export', items: [
      { id: 'csv', text: 'CSV' },
      { id: 'json', text: 'JSON' },
    ]},
  ]}
>
  Export
</ButtonDropdown>
```

## Link

```jsx
import { Link } from '@cloudscape-design/components';

// Internal link
<Link href="/resources">View resources</Link>

// External link (opens in new tab)
<Link href="https://docs.example.com" external>Documentation</Link>

// Info link (triggers help panel)
<Link variant="info" onFollow={() => setToolsOpen(true)}>Info</Link>

// Primary link (for tables/cards)
<Link variant="primary" href={`/resources/${item.id}`}>{item.name}</Link>
```

## Pagination

```jsx
import { Pagination } from '@cloudscape-design/components';

<Pagination
  currentPageIndex={currentPage}
  pagesCount={Math.ceil(totalItems / pageSize)}
  onChange={({ detail }) => setCurrentPage(detail.currentPageIndex)}
  ariaLabels={{
    nextPageLabel: 'Next page',
    previousPageLabel: 'Previous page',
    pageLabel: pageNumber => `Page ${pageNumber}`,
  }}
/>
```

## TopNavigation

```jsx
import { TopNavigation } from '@cloudscape-design/components';

<TopNavigation
  identity={{
    href: '/',
    title: 'My Application',
    logo: { src: '/logo.png', alt: 'Logo' }
  }}
  utilities={[
    { type: 'button', text: 'Help', onClick: () => setToolsOpen(true) },
    {
      type: 'menu-dropdown',
      text: 'User',
      iconName: 'user-profile',
      items: [
        { id: 'profile', text: 'Profile' },
        { id: 'preferences', text: 'Preferences' },
        { id: 'signout', text: 'Sign out' },
      ],
      onItemClick: ({ detail }) => handleUserAction(detail.id)
    }
  ]}
/>
```

## Wizard

For multi-step forms:

```jsx
import { Wizard } from '@cloudscape-design/components';

<Wizard
  activeStepIndex={activeStep}
  onNavigate={({ detail }) => setActiveStep(detail.requestedStepIndex)}
  onCancel={() => navigate('/resources')}
  onSubmit={handleSubmit}
  steps={[
    {
      title: 'Choose type',
      content: <Step1Content />,
      isOptional: false,
    },
    {
      title: 'Configure settings',
      content: <Step2Content />,
    },
    {
      title: 'Review and create',
      content: <ReviewContent />,
    },
  ]}
  i18nStrings={{
    stepNumberLabel: stepNumber => `Step ${stepNumber}`,
    collapsedStepsLabel: (stepNumber, stepsCount) => `Step ${stepNumber} of ${stepsCount}`,
    cancelButton: 'Cancel',
    previousButton: 'Previous',
    nextButton: 'Next',
    submitButton: 'Create resource',
  }}
/>
```

## Best Practices

1. Use `primary` button variant sparingly (one per section)
2. Keep navigation structure simple and intuitive
3. Highlight current page with `activeHref`
4. Use breadcrumbs for deep navigation hierarchies
5. Provide `ariaLabel` for icon-only buttons
6. Use `formAction="none"` on Cancel buttons in forms
7. Group related buttons with `SpaceBetween direction="horizontal" size="xs"`
