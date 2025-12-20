# Cloudscape Layout Patterns

Page structure, containers, grids, and spacing components.

## Application Layout Types

### Standard AppLayout

For general use cases with collapsible navigation, tools panel, and content area.

```jsx
import {
  AppLayout,
  SideNavigation,
  ContentLayout,
  Container,
  Header,
  BreadcrumbGroup,
  Flashbar
} from '@cloudscape-design/components';

function StandardLayout() {
  const [navigationOpen, setNavigationOpen] = useState(true);
  const [toolsOpen, setToolsOpen] = useState(false);

  return (
    <AppLayout
      navigation={
        <SideNavigation
          activeHref="#"
          header={{ text: 'My App', href: '#' }}
          items={[
            { type: 'link', text: 'Dashboard', href: '/dashboard' },
            { type: 'section', text: 'Resources', items: [
              { type: 'link', text: 'Instances', href: '/instances' },
              { type: 'link', text: 'Storage', href: '/storage' },
            ]}
          ]}
        />
      }
      navigationOpen={navigationOpen}
      onNavigationChange={({ detail }) => setNavigationOpen(detail.open)}

      breadcrumbs={
        <BreadcrumbGroup items={[
          { text: 'Home', href: '/' },
          { text: 'Resources', href: '/resources' },
        ]} />
      }

      notifications={<Flashbar items={notifications} />}

      content={
        <ContentLayout header={<Header variant="h1">Page Title</Header>}>
          <Container>Main content</Container>
        </ContentLayout>
      }

      tools={<HelpPanel header={<h2>Help</h2>}>Help content</HelpPanel>}
      toolsOpen={toolsOpen}
      onToolsChange={({ detail }) => setToolsOpen(detail.open)}

      contentType="default"

      ariaLabels={{
        navigation: 'Side navigation',
        navigationClose: 'Close navigation',
        navigationToggle: 'Open navigation',
        tools: 'Help panel',
        toolsClose: 'Close help',
        toolsToggle: 'Open help'
      }}
    />
  );
}
```

### Content Types

Set `contentType` to optimize layout for specific patterns:

| Type | Use Case | Max Width |
|------|----------|-----------|
| `default` | General pages, details, dashboards | Fixed |
| `form` | Create/edit forms | Fixed |
| `wizard` | Multi-step wizards | Fixed |
| `table` | Full-page tables | 100% |
| `cards` | Full-page card collections | 100% |
| `dashboard` | Multi-column dashboards | 100% |

### AppLayout with Split Panel

For detail views with item selection:

```jsx
<AppLayout
  content={<Table selectionType="single" onSelectionChange={handleSelect} />}

  splitPanel={
    <SplitPanel header={`Details: ${selectedItem?.name}`}>
      <KeyValuePairs items={[
        { label: 'ID', value: selectedItem?.id },
        { label: 'Status', value: selectedItem?.status },
      ]} />
    </SplitPanel>
  }
  splitPanelOpen={splitPanelOpen}
  onSplitPanelToggle={({ detail }) => setSplitPanelOpen(detail.open)}
  splitPanelSize={300}
  onSplitPanelResize={({ detail }) => setSplitPanelSize(detail.size)}
/>
```

## ContentLayout

For expressive pages with hero headers or centered content:

```jsx
import { ContentLayout, Container, Header } from '@cloudscape-design/components';

// Basic usage
<ContentLayout
  header={<Header variant="h1">Page Title</Header>}
>
  <Container>Content here</Container>
</ContentLayout>

// With hero header styling
<ContentLayout
  headerVariant="high-contrast"
  headerBackgroundStyle="linear-gradient(135deg, #232f3e 0%, #0073bb 100%)"
  header={
    <Header variant="h1" description="Welcome to the dashboard">
      My Application
    </Header>
  }
>
  <Container>Content overlaps header by default</Container>
</ContentLayout>

// Disable overlap for non-container content
<ContentLayout
  disableOverlap={true}
  header={<Header variant="h1">Documentation</Header>}
>
  <TextContent>
    <p>Text content without container</p>
  </TextContent>
</ContentLayout>
```

### ContentLayout Variants

| Variant | Use |
|---------|-----|
| `default` | No additional styling |
| `divider` | Divider between header and content |
| `high-contrast` | Dark visual context for header |

## Container

Groups related content visually:

```jsx
import { Container, Header, SpaceBetween } from '@cloudscape-design/components';

// Basic container
<Container header={<Header variant="h2">Settings</Header>}>
  Content here
</Container>

// Container with footer
<Container
  header={<Header variant="h2">Configuration</Header>}
  footer={
    <ExpandableSection headerText="Advanced options">
      Advanced content
    </ExpandableSection>
  }
>
  Main content
</Container>

// Stacked containers (optimized for adjacent display)
<SpaceBetween size="l">
  <Container variant="stacked" header={<Header variant="h2">Section 1</Header>}>
    Content 1
  </Container>
  <Container variant="stacked" header={<Header variant="h2">Section 2</Header>}>
    Content 2
  </Container>
</SpaceBetween>

// Container with media
<Container
  media={{
    content: <img src="/image.jpg" alt="Description" />,
    position: 'top',
    height: 200
  }}
  header={<Header variant="h2">Featured</Header>}
>
  Content below image
</Container>
```

## Grid Systems

### ColumnLayout

For equal-width columns:

```jsx
import { ColumnLayout, Container } from '@cloudscape-design/components';

// 2 columns
<ColumnLayout columns={2}>
  <Container>Column 1</Container>
  <Container>Column 2</Container>
</ColumnLayout>

// 3 columns with borders
<ColumnLayout columns={3} borders="vertical">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</ColumnLayout>

// 4 columns for key-value pairs
<ColumnLayout columns={4} variant="text-grid">
  <SpaceBetween size="xs">
    <Box variant="awsui-key-label">Status</Box>
    <StatusIndicator type="success">Running</StatusIndicator>
  </SpaceBetween>
  <SpaceBetween size="xs">
    <Box variant="awsui-key-label">Type</Box>
    <Box>t2.micro</Box>
  </SpaceBetween>
</ColumnLayout>
```

### Grid (12-column system)

For custom column widths:

```jsx
import { Grid, Container } from '@cloudscape-design/components';

// 8-4 split
<Grid gridDefinition={[{ colspan: 8 }, { colspan: 4 }]}>
  <Container>Main content (8 cols)</Container>
  <Container>Sidebar (4 cols)</Container>
</Grid>

// Responsive grid
<Grid gridDefinition={[
  { colspan: { default: 12, s: 6, l: 4 } },
  { colspan: { default: 12, s: 6, l: 4 } },
  { colspan: { default: 12, s: 12, l: 4 } }
]}>
  <Container>Card 1</Container>
  <Container>Card 2</Container>
  <Container>Card 3</Container>
</Grid>

// Dashboard layout
<Grid gridDefinition={[
  { colspan: 12 },           // Full width header
  { colspan: 8 }, { colspan: 4 },  // Main + sidebar
  { colspan: 4 }, { colspan: 4 }, { colspan: 4 }  // 3 equal cards
]}>
  <Container>Header section</Container>
  <Container>Main chart</Container>
  <Container>Summary</Container>
  <Container>Metric 1</Container>
  <Container>Metric 2</Container>
  <Container>Metric 3</Container>
</Grid>
```

## SpaceBetween

Consistent spacing between elements:

```jsx
import { SpaceBetween, Button, FormField, Input, Container } from '@cloudscape-design/components';

// Horizontal button group
<SpaceBetween direction="horizontal" size="xs">
  <Button variant="link">Cancel</Button>
  <Button variant="normal">Reset</Button>
  <Button variant="primary">Submit</Button>
</SpaceBetween>

// Vertical form fields
<SpaceBetween direction="vertical" size="l">
  <FormField label="Name"><Input /></FormField>
  <FormField label="Email"><Input type="email" /></FormField>
  <FormField label="Phone"><Input type="tel" /></FormField>
</SpaceBetween>

// Stacked containers
<SpaceBetween size="l">
  <Container header={<Header variant="h2">Section 1</Header>}>Content</Container>
  <Container header={<Header variant="h2">Section 2</Header>}>Content</Container>
</SpaceBetween>

// Nested spacing
<SpaceBetween direction="horizontal" size="l">
  <SpaceBetween size="xs">
    <div>Group 1 - Item 1</div>
    <div>Group 1 - Item 2</div>
  </SpaceBetween>
  <SpaceBetween size="xs">
    <div>Group 2 - Item 1</div>
    <div>Group 2 - Item 2</div>
  </SpaceBetween>
</SpaceBetween>
```

### SpaceBetween Sizes

| Size | Pixels | Use Case |
|------|--------|----------|
| `xxxs` | 2px | Minimal spacing |
| `xxs` | 4px | Label to control |
| `xs` | 8px | Buttons in action stripe |
| `s` | 12px | Related items |
| `m` | 16px | Cards, medium separation |
| `l` | 20px | Form fields, containers |
| `xl` | 24px | Major sections |
| `xxl` | 32px | Large separation |

## Box

Custom padding, margin, and display:

```jsx
import { Box } from '@cloudscape-design/components';

// Padding and margin
<Box padding="l" margin={{ top: 'm', bottom: 's' }}>
  Content with custom spacing
</Box>

// Text variants
<Box variant="h2">Heading 2</Box>
<Box variant="p">Paragraph text</Box>
<Box variant="small">Small text</Box>
<Box variant="code">code_snippet</Box>
<Box variant="awsui-key-label">Label</Box>

// Colors
<Box color="text-status-error">Error text</Box>
<Box color="text-status-success">Success text</Box>
<Box color="text-body-secondary">Secondary text</Box>

// Display and alignment
<Box display="flex" float="right">
  <Button>Action</Button>
</Box>

// Font styling
<Box fontSize="heading-m" fontWeight="bold">
  Custom styled text
</Box>
```

## Common Layout Patterns

### Detail Page

```jsx
<AppLayout
  content={
    <ContentLayout header={<Header variant="h1">Resource Details</Header>}>
      <SpaceBetween size="l">
        <Container header={<Header variant="h2">Overview</Header>}>
          <ColumnLayout columns={4} variant="text-grid">
            <KeyValuePairs items={overviewItems} />
          </ColumnLayout>
        </Container>

        <Container header={<Header variant="h2">Configuration</Header>}>
          <KeyValuePairs items={configItems} />
        </Container>

        <Table
          header={<Header variant="h2">Related Resources</Header>}
          items={relatedItems}
          columnDefinitions={columns}
        />
      </SpaceBetween>
    </ContentLayout>
  }
/>
```

### Form Page

```jsx
<AppLayout
  contentType="form"
  content={
    <Form
      header={<Header variant="h1">Create Resource</Header>}
      actions={
        <SpaceBetween direction="horizontal" size="xs">
          <Button variant="link">Cancel</Button>
          <Button variant="primary">Create</Button>
        </SpaceBetween>
      }
    >
      <SpaceBetween size="l">
        <Container header={<Header variant="h2">Settings</Header>}>
          <SpaceBetween size="l">
            <FormField label="Name"><Input /></FormField>
            <FormField label="Description"><Textarea /></FormField>
          </SpaceBetween>
        </Container>

        <Container header={<Header variant="h2">Configuration</Header>}>
          <SpaceBetween size="l">
            <FormField label="Type"><Select options={typeOptions} /></FormField>
            <FormField label="Region"><Select options={regionOptions} /></FormField>
          </SpaceBetween>
        </Container>
      </SpaceBetween>
    </Form>
  }
/>
```

### Dashboard Page

```jsx
<AppLayout
  contentType="dashboard"
  content={
    <ContentLayout header={<Header variant="h1">Dashboard</Header>}>
      <Grid gridDefinition={[
        { colspan: 8 }, { colspan: 4 },
        { colspan: 4 }, { colspan: 4 }, { colspan: 4 }
      ]}>
        <Container header={<Header variant="h2">Trends</Header>}>
          <LineChart series={trendData} />
        </Container>

        <Container header={<Header variant="h2">Summary</Header>}>
          <KeyValuePairs items={summaryItems} />
        </Container>

        <Container header={<Header variant="h2">Metric 1</Header>}>
          <Box variant="h1" textAlign="center">1,234</Box>
        </Container>

        <Container header={<Header variant="h2">Metric 2</Header>}>
          <Box variant="h1" textAlign="center">567</Box>
        </Container>

        <Container header={<Header variant="h2">Metric 3</Header>}>
          <Box variant="h1" textAlign="center">89%</Box>
        </Container>
      </Grid>
    </ContentLayout>
  }
/>
```

## Best Practices

1. Use only one AppLayout per page
2. Set appropriate `contentType` for layout optimization
3. Use `SpaceBetween` for consistent spacing (not manual margins)
4. Use `Container` to group related content
5. Use `ColumnLayout` for equal columns, `Grid` for custom widths
6. Keep navigation concise and well-organized
7. Provide breadcrumbs for navigation hierarchy
8. Use `ContentLayout` inside `AppLayout.content` for page structure
