---
name: "cloudscape"
displayName: "Cloudscape Design System"
description: "Build frontend React applications with Cloudscape's accessible, responsive design components"
author: "Praveen Chamarthi"
keywords: ["cloudscape", "aws-ui", "react", "frontend", "component", "form", "table", "layout", "navigation", "design-system", "charts", "data-visualization", "genai", "theming", "design-tokens"]
---

# Cloudscape Design System Power

Build professional, accessible React applications using the Cloudscape Design System.

## Overview

Cloudscape is an open-source design system created by AWS for building intuitive, engaging, and inclusive user experiences at scale. It powers the AWS Management Console and provides 90+ tested, responsive, accessible React components.

**Key capabilities:**

- 90+ production-ready React components with built-in accessibility (WCAG 2.1 AA)
- Token-based theming with light/dark mode and content density settings
- Comprehensive layout system (AppLayout, Grid, ColumnLayout)
- Data-intensive patterns (Tables, Cards, PropertyFilter, Pagination)
- GenAI components (ChatBubble, PromptInput, Avatar, LoadingBar)
- 60+ UX pattern guidelines backed by user research

**Perfect for:**

- AWS console-style applications and admin dashboards
- Internal tools and data-intensive applications
- Applications requiring strong accessibility compliance
- GenAI chat interfaces and AI-powered features
- Any React application needing a professional, consistent design system

## Available Steering Files

This power has the following steering files:

- **foundations.md** - Design tokens, spacing system, colors, typography, theming, visual modes
- **layout-patterns.md** - AppLayout, ContentLayout, Container, Grid, ColumnLayout, SpaceBetween, Box
- **form-patterns.md** - Form, FormField, Input, Select, Multiselect, validation patterns
- **table-and-collections.md** - Table, Cards, filtering, pagination, useCollection hook
- **navigation-patterns.md** - SideNavigation, Tabs, BreadcrumbGroup, Button, Link, Wizard
- **feedback-patterns.md** - Alert, Flashbar, Modal, StatusIndicator, ProgressBar, Spinner
- **charts-and-data-viz.md** - LineChart, BarChart, PieChart, data visualization patterns
- **genai-patterns.md** - Avatar, ChatBubble, PromptInput, LoadingBar for AI interfaces

## Onboarding

### Step 1: Validate Dependencies

Before using Cloudscape, ensure your project has the required packages:

```bash
# Core packages (required)
npm install @cloudscape-design/components
npm install @cloudscape-design/global-styles

# Optional but recommended
npm install @cloudscape-design/design-tokens
npm install @cloudscape-design/collection-hooks
```

### Step 2: Setup Global Styles

Import global styles ONCE in your app entry point:

```jsx
// In your main App.jsx or index.jsx
import '@cloudscape-design/global-styles/index.css';
```

## Core Patterns

### Essential Imports

```jsx
// Global styles (once per app)
import '@cloudscape-design/global-styles/index.css';

// Components - import individually for better tree-shaking
import Button from '@cloudscape-design/components/button';
import Container from '@cloudscape-design/components/container';
import Header from '@cloudscape-design/components/header';

// Or grouped import
import { Button, Container, Header, Input, Form, FormField, SpaceBetween } from '@cloudscape-design/components';

// Design tokens
import * as awsui from '@cloudscape-design/design-tokens/index.js';

// Visual modes
import { applyMode, applyDensity, Mode, Density } from '@cloudscape-design/global-styles';

// Collection hooks for tables/cards
import { useCollection } from '@cloudscape-design/collection-hooks';
```

### Event Handling Pattern

All Cloudscape components use a consistent `{ detail }` event pattern:

```jsx
// Input
<Input value={value} onChange={({ detail }) => setValue(detail.value)} />

// Checkbox
<Checkbox checked={checked} onChange={({ detail }) => setChecked(detail.checked)} />

// Select
<Select selectedOption={option} onChange={({ detail }) => setOption(detail.selectedOption)} />

// Table selection
<Table selectedItems={selected} onSelectionChange={({ detail }) => setSelected(detail.selectedItems)} />
```

### Controlled Components

All form components are controlled - always manage state explicitly:

```jsx
const [name, setName] = useState('');
const [role, setRole] = useState({ label: 'User', value: 'user' });

<Input value={name} onChange={({ detail }) => setName(detail.value)} />
<Select
  selectedOption={role}
  options={[{ label: 'User', value: 'user' }, { label: 'Admin', value: 'admin' }]}
  onChange={({ detail }) => setRole(detail.selectedOption)}
/>
```

### Visual Modes

```jsx
import { applyMode, applyDensity, Mode, Density } from '@cloudscape-design/global-styles';

// Light/Dark mode
applyMode(Mode.Dark);
applyMode(Mode.Light);

// Content density
applyDensity(Density.Compact);    // Data-intensive views
applyDensity(Density.Comfortable); // Default, better readability
```

## Component Categories

| Category | Components |
|----------|------------|
| Layout | AppLayout, ContentLayout, Container, Grid, ColumnLayout, Box, SpaceBetween |
| Navigation | SideNavigation, BreadcrumbGroup, Tabs, Button, Link, Pagination, TopNavigation |
| Forms | Form, FormField, Input, Select, Multiselect, Checkbox, RadioGroup, Toggle, DatePicker |
| Data | Table, Cards, PropertyFilter, TextFilter, CollectionPreferences |
| Feedback | Alert, Flashbar, Modal, StatusIndicator, ProgressBar, Spinner, Popover |
| Charts | LineChart, BarChart, AreaChart, PieChart, MixedLineBarChart |
| GenAI | Avatar, ChatBubble, PromptInput, LoadingBar, SupportPromptGroup |

## Best Practices

### ✅ Do

- Import global styles ONCE in your app entry point before any components
- Use design tokens for all colors, spacing, and typography - never hard-code values
- Use the `{ detail }` event pattern for all component events
- Wrap all form inputs with `FormField` for proper labeling and accessibility
- Use `SpaceBetween` for consistent spacing between elements (not manual margins)
- Set appropriate `contentType` on AppLayout (table, form, wizard, dashboard, default)
- Provide `ariaLabels` on interactive components (Table, AppLayout, Modal)
- Test in both light and dark modes
- Use `useCollection` hook for tables with filtering, sorting, and pagination
- Store and restore user preferences (column widths, page size, visible columns)

### ❌ Don't

- Hard-code colors, spacing, or font values - use design tokens
- Use multiple `AppLayout` components per page - only one allowed
- Nest `AppLayout` components
- Break built-in accessibility features
- Use `h1` Header variant more than once per page
- Skip the `ariaLabels` prop on Table, AppLayout, or Modal
- Use `onChange={(e) => setValue(e.target.value)}` - use `({ detail }) => setValue(detail.value)`
- Add unnecessary ARIA roles or landmarks - components handle this
- Use `formAction="submit"` on Cancel buttons - use `formAction="none"`

## Troubleshooting

### Error: "Styles not loading / components look unstyled"

**Cause:** Global styles not imported or imported after components render

**Solution:** Add to your app entry point (App.jsx or index.jsx) BEFORE any component imports:

```jsx
import '@cloudscape-design/global-styles/index.css';
```

### Error: "onChange not firing / value not updating"

**Cause:** Using wrong event pattern (React native vs Cloudscape)

**Solution:** Use `{ detail }` destructuring pattern:

```jsx
// ❌ Wrong - React native pattern
onChange={(e) => setValue(e.target.value)}

// ✅ Correct - Cloudscape pattern
onChange={({ detail }) => setValue(detail.value)}
```

### Error: "Dark mode not working"

**Cause:** Missing `applyMode` call or called too late

**Solution:** Call `applyMode` early in your app initialization:

```jsx
import { applyMode, Mode } from '@cloudscape-design/global-styles';

// Call before rendering components
applyMode(Mode.Dark);
```

### Error: "Table selection not working"

**Cause:** Missing `selectionType` or `ariaLabels` props

**Solution:** Add both required props:

```jsx
<Table
  selectionType="multi"  // or "single"
  selectedItems={selectedItems}
  onSelectionChange={({ detail }) => setSelectedItems(detail.selectedItems)}
  ariaLabels={{
    selectionGroupLabel: 'Item selection',
    allItemsSelectionLabel: 'Select all',
    itemSelectionLabel: (data, row) => `Select ${row.name}`
  }}
/>
```

### Error: "Form validation not showing errors"

**Cause:** Missing `errorText` prop on FormField or `invalid` prop on Input

**Solution:** Set both props when validation fails:

```jsx
<FormField label="Email" errorText={errors.email}>
  <Input
    type="email"
    value={email}
    invalid={!!errors.email}
    onChange={({ detail }) => setEmail(detail.value)}
  />
</FormField>
```

### Error: "AppLayout content overlapping header/footer"

**Cause:** Custom header/footer not configured in AppLayout

**Solution:** Specify header/footer selectors:

```jsx
<AppLayout
  headerSelector="#custom-header"
  footerSelector="#custom-footer"
  content={...}
/>
```

### Error: "Table columns not resizing properly"

**Cause:** Missing `width` values with `resizableColumns` enabled

**Solution:** Define explicit `width` and `minWidth` for all columns:

```jsx
columnDefinitions={[
  { id: 'name', header: 'Name', cell: item => item.name, width: 200, minWidth: 150 },
  { id: 'status', header: 'Status', cell: item => item.status, width: 120, minWidth: 100 },
]}
```

## Tips

1. **Start with AppLayout** - Use it as the root layout component for consistent page structure
2. **Use ContentLayout inside AppLayout** - For proper page headers and content spacing with `disableContentPaddings={true}`
3. **Set contentType** - Match it to your page type for optimized layout (table, form, wizard, dashboard)
4. **Leverage useCollection hook** - Simplifies table state management for filtering, sorting, pagination
5. **Use StatusIndicator for states** - Not colored text or custom badges
6. **Show item counts in headers** - Format: `(150)` or `(5/150)` when items selected
7. **Use `-` for empty cells** - Consistent empty value representation in tables
8. **Test with keyboard** - All components should be fully keyboard navigable
9. **Check component playground** - Each component has interactive examples at cloudscape.design

## When to Load Steering Files

- Working with design tokens, spacing, colors, typography, theming → `foundations.md`
- Setting up app layout, page structure, containers, grids → `layout-patterns.md`
- Building forms, inputs, validation, select components → `form-patterns.md`
- Implementing tables, cards, filtering, pagination → `table-and-collections.md`
- Adding navigation (sidebar, tabs, breadcrumbs, buttons) → `navigation-patterns.md`
- Using alerts, notifications, modals, status indicators → `feedback-patterns.md`
- Creating charts and data visualizations → `charts-and-data-viz.md`
- Building GenAI interfaces (chat, prompts, AI indicators) → `genai-patterns.md`
