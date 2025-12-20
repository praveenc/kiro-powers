# Cloudscape Foundations

Design tokens, spacing, colors, typography, and theming fundamentals.

## Design Tokens

Design tokens are key-value pairs representing reusable design decisions. Use tokens instead of hard-coded values for visual mode compliance.

### Token Categories

| Category | Example | Purpose |
|----------|---------|---------|
| Color | `$color-text-body-default` | Text, backgrounds, borders |
| Spacing | `$space-scaled-m` | Paddings, margins |
| Typography | `$font-family-base` | Font families, sizes |
| Border | `$border-radius-container` | Border radii |
| Motion | `$motion-duration-responsive` | Animation timing |

### Usage

```scss
// Sass
@use '@cloudscape-design/design-tokens/index.scss' as awsui;

.custom-panel {
  color: awsui.$color-text-body-secondary;
  padding: awsui.$space-scaled-l;
  border-radius: awsui.$border-radius-container;
}
```

```jsx
// JavaScript
import * as awsui from '@cloudscape-design/design-tokens/index.js';

const styles = {
  color: awsui.colorTextBodySecondary,
  padding: awsui.spaceScaledL,
  borderRadius: awsui.borderRadiusContainer
};
```

## Spacing System

Based on a 4px grid system with progressive increments.

### Spacing Scale

| Token | Size | Use Case |
|-------|------|----------|
| `xxx-small` | 2px | Container header to description |
| `xx-small` | 4px | Form field label to control |
| `x-small` | 8px | Buttons in action stripe, checkbox to label |
| `small` | 12px | Vertical padding in popover |
| `medium` | 16px | Horizontal padding in popover, space between cards |
| `large` | 20px | Grid gutter, container padding, space between form fields |
| `x-large` | 24px | Tutorial panel tasks |
| `xx-large` | 32px | Grouped options indentation |
| `xxx-large` | 40px | App layout padding (large screens) |

### Scaled vs Static Tokens

- **Scaled** (`$space-scaled-*`): Change between comfortable/compact modes
- **Static** (`$space-static-*`): Remain constant across modes

### Implementation

```jsx
import { Box, SpaceBetween } from '@cloudscape-design/components';

// SpaceBetween for consistent spacing between elements
<SpaceBetween size="l" direction="vertical">
  <FormField label="Name"><Input /></FormField>
  <FormField label="Email"><Input /></FormField>
</SpaceBetween>

// Box for custom padding/margin
<Box padding="l" margin={{ top: 'm' }}>
  Content with custom spacing
</Box>
```

## Colors

### Philosophy

- Most UI is grayscale - creates contrast for important elements
- Blue for primary actions, links, interactive elements
- Status colors: Red (error), Green (success), Yellow (warning), Grey (neutral)

### Key Color Tokens

| Purpose | Token Pattern | Usage |
|---------|---------------|-------|
| Body text | `colorTextBodyDefault` | Default text |
| Secondary text | `colorTextBodySecondary` | Descriptions, placeholders |
| Interactive | `colorTextInteractiveDefault` | Links, buttons |
| Status | `colorTextStatus{Error,Success,Warning,Info}` | Status indicators |
| Background | `colorBackgroundLayout{Main,Toggle}` | Page backgrounds |
| Container | `colorBackgroundContainer{Content,Header}` | Container areas |

### Status Indicators

```jsx
<StatusIndicator type="success">Running</StatusIndicator>
<StatusIndicator type="error">Failed</StatusIndicator>
<StatusIndicator type="warning">At risk</StatusIndicator>
<StatusIndicator type="info">In progress</StatusIndicator>
<StatusIndicator type="pending">Pending</StatusIndicator>
```

### Rules

- Never use color alone - pair with icons or text
- Ensure 3:1 contrast minimum
- Use design tokens, not hex values
- Test in both light and dark modes

## Typography

### Typefaces

- **Primary**: Open Sans (Light 300, Normal 400, Bold 700)
- **Monospace**: Monaco, Menlo, Consolas (code, IDs, numeric data)

### Type Hierarchy

```jsx
<Header variant="h1">Page Title</Header>      // 24px, Bold
<Header variant="h2">Section</Header>         // 20px, Bold
<Header variant="h3">Subsection</Header>      // 18px, Bold
<Box variant="h4">Minor heading</Box>         // 16px, Bold
<Box variant="p">Body text</Box>              // 14px, Normal
<Box variant="small">Small text</Box>         // 12px, Normal
```

### Typography Tokens

| Token | Value | Usage |
|-------|-------|-------|
| `fontFamilyBase` | Open Sans | All UI text |
| `fontFamilyMonospace` | Monaco, Menlo | Code, IDs |
| `fontSizeBodyM` | 14px | Default body |
| `fontSizeBodyS` | 12px | Small text, descriptions |
| `fontSizeHeadingXl` | 24px | h1 |
| `fontSizeHeadingL` | 20px | h2 |
| `fontSizeHeadingM` | 18px | h3 |

### Guidelines

- Use sentence case (not title case)
- One `h1` per page
- Use headings for hierarchy, not visual appearance
- Minimum font size: 12px
- Use monospace sparingly

### Implementation

```jsx
import { Box, TextContent } from '@cloudscape-design/components';

// Single element styling
<Box variant="h2" color="text-status-error">Error Title</Box>
<Box variant="code">instance-id-12345</Box>

// Chunk of HTML content
<TextContent>
  <h2>Section Title</h2>
  <p>Body text with <code>inline code</code>.</p>
  <ul>
    <li>List item one</li>
    <li>List item two</li>
  </ul>
</TextContent>
```

## Visual Modes

### Light/Dark Mode

```jsx
import { applyMode, Mode } from '@cloudscape-design/global-styles';

applyMode(Mode.Dark);
applyMode(Mode.Light);
```

### Content Density

```jsx
import { applyDensity, Density } from '@cloudscape-design/global-styles';

// Compact: reduced spacing, more data visibility
applyDensity(Density.Compact);

// Comfortable: standard spacing, better readability (default)
applyDensity(Density.Comfortable);
```

### Guidelines

- Default to Comfortable mode
- Allow users to switch between modes
- Apply consistently across entire service
- Test in both light and dark modes

## Theming

### Runtime Theming

```jsx
import { applyTheme } from '@cloudscape-design/components/theming';

const theme = {
  tokens: {
    // Typography
    fontFamilyBase: "'Helvetica Neue', Roboto, Arial, sans-serif",

    // Border radius
    borderRadiusButton: '4px',
    borderRadiusContainer: '8px',

    // Colors (light/dark)
    colorBackgroundLayoutMain: {
      light: '#ffffff',
      dark: '#0f1b2a'
    },
    // Same value for both modes
    colorTextAccent: '#0073bb',
  },
  contexts: {
    'top-navigation': {
      tokens: {
        colorTextAccent: '#44b9d6',
      },
    },
  },
};

const { reset } = applyTheme({ theme });
// Call reset() to remove custom theme
```

### Themeable Token Categories

- **Typography**: `fontFamilyBase`, `fontFamilyMonospace`
- **Colors**: Most color tokens (check docs for specific ones)
- **Border radius**: `borderRadiusButton`, `borderRadiusContainer`, `borderRadiusInput`, etc.

### Not Themeable

- Spacing tokens
- Motion tokens
- Iconography
- Individual component instances

## Motion

### Easing Curves

| Curve | Value | Use |
|-------|-------|-----|
| Responsive | `cubic-bezier(0, 0, 0, 1)` | Quick, smooth feedback |
| Sticky | `cubic-bezier(1, 0, 0.83, 1)` | Sticky to state |
| Expressive | `cubic-bezier(0.84, 0, 0.16, 1)` | Draw attention |

### Duration

| Token | Value | Use |
|-------|-------|-----|
| `motionDurationResponsive` | 115ms | Quick, responsive |
| `motionDurationExpressive` | 165ms | More expressive |
| `motionDurationComplex` | 250ms | Draw attention, complexity |

### Disable Motion

```jsx
import { disableMotion } from '@cloudscape-design/global-styles';

disableMotion(true); // For tests or user preference (prefers-reduced-motion)
```

## Quick Reference

### Essential Token Imports

```jsx
// Design tokens
import * as awsui from '@cloudscape-design/design-tokens/index.js';

// Common tokens
awsui.colorTextBodyDefault        // Default text color
awsui.colorBackgroundLayoutMain   // Page background
awsui.spaceScaledL                // Large spacing (20px comfortable)
awsui.fontSizeBodyM               // Body font size (14px)
awsui.borderRadiusContainer       // Container border radius
```

### Spacing Components

```jsx
// Vertical spacing between form fields
<SpaceBetween size="l" direction="vertical">

// Horizontal spacing between buttons
<SpaceBetween size="xs" direction="horizontal">

// Custom box spacing
<Box padding={{ horizontal: 'l', vertical: 'm' }} margin={{ top: 's' }}>
```
