# Cloudscape Charts and Data Visualization

Line charts, bar charts, pie charts, and data visualization patterns.

## Chart Types

| Type | Use Case |
|------|----------|
| LineChart | Trends over time |
| BarChart | Comparisons, categorical data |
| AreaChart | Part-to-whole over time (stacked) |
| PieChart/DonutChart | Proportions of a whole |

## LineChart

```jsx
import { LineChart, Container, Header, Box } from '@cloudscape-design/components';

<Container header={<Header variant="h2">CPU Utilization</Header>}>
  <LineChart
    series={[
      {
        title: 'Instance 1',
        type: 'line',
        data: [
          { x: new Date('2024-01-01'), y: 15 },
          { x: new Date('2024-01-02'), y: 25 },
          { x: new Date('2024-01-03'), y: 20 },
        ]
      },
      {
        title: 'Instance 2',
        type: 'line',
        data: [
          { x: new Date('2024-01-01'), y: 30 },
          { x: new Date('2024-01-02'), y: 35 },
          { x: new Date('2024-01-03'), y: 28 },
        ]
      }
    ]}
    xDomain={[new Date('2024-01-01'), new Date('2024-01-03')]}
    yDomain={[0, 100]}
    xTitle="Date"
    yTitle="CPU %"
    height={300}
    hideFilter
    ariaLabel="CPU utilization chart"
    ariaDescription="Line chart showing CPU usage over time"
    i18nStrings={{
      legendAriaLabel: 'Legend',
      chartAriaRoleDescription: 'line chart',
    }}
    empty={<Box textAlign="center">No data</Box>}
    noMatch={<Box textAlign="center">No matching data</Box>}
  />
</Container>
```

## BarChart

```jsx
import { BarChart } from '@cloudscape-design/components';

// Vertical bars (default)
<BarChart
  series={[
    {
      title: 'Errors',
      type: 'bar',
      data: [
        { x: 'Service A', y: 10 },
        { x: 'Service B', y: 25 },
        { x: 'Service C', y: 15 },
      ]
    }
  ]}
  xTitle="Service"
  yTitle="Error count"
  height={300}
/>

// Horizontal bars
<BarChart
  series={[...]}
  horizontalBars
/>

// Stacked bars
<BarChart
  series={[
    { title: 'Success', type: 'bar', data: [...] },
    { title: 'Errors', type: 'bar', data: [...] },
  ]}
  stackedBars
/>
```

## PieChart / DonutChart

```jsx
import { PieChart } from '@cloudscape-design/components';

// Pie chart
<PieChart
  data={[
    { title: 'Running', value: 60, color: '#1d8102' },
    { title: 'Stopped', value: 30, color: '#d13212' },
    { title: 'Pending', value: 10, color: '#879596' },
  ]}
  detailPopoverContent={(datum, sum) => [
    { key: 'Count', value: datum.value },
    { key: 'Percentage', value: `${((datum.value / sum) * 100).toFixed(0)}%` },
  ]}
  ariaLabel="Instance status distribution"
  ariaDescription="Pie chart showing instance status"
/>

// Donut chart (with inner metric)
<PieChart
  data={data}
  variant="donut"
  innerMetricDescription="Total"
  innerMetricValue="100"
/>
```

## AreaChart

```jsx
import { AreaChart } from '@cloudscape-design/components';

<AreaChart
  series={[
    { title: 'Free tier', type: 'area', data: [...] },
    { title: 'Pro', type: 'area', data: [...] },
    { title: 'Enterprise', type: 'area', data: [...] },
  ]}
  xScaleType="time"
  yTitle="Active users"
  stackedBars  // For stacked area
/>
```

## Chart States

```jsx
<LineChart
  series={series}
  // Loading state
  loading={isLoading}
  loadingText="Loading chart data"

  // Empty state
  empty={
    <Box textAlign="center" color="inherit">
      <b>No data available</b>
      <Box variant="p" color="inherit">There is no data to display.</Box>
    </Box>
  }

  // No match state (after filtering)
  noMatch={
    <Box textAlign="center" color="inherit">
      <b>No matching data</b>
      <Box variant="p" color="inherit">No data matches the filter criteria.</Box>
      <Button onClick={clearFilters}>Clear filters</Button>
    </Box>
  }

  // Error state
  errorText="Failed to load chart data"
  recoveryText="Retry"
  onRecoveryClick={handleRetry}
/>
```

## Chart Features

### Legend & Filtering

```jsx
<LineChart
  series={series}
  // Built-in legend with filtering
  hideLegend={false}
  legendTitle="Metrics"

  // Or hide and use external filter
  hideFilter
/>
```

### Thresholds

```jsx
<LineChart
  series={series}
  additionalFilters={
    <Checkbox checked={showThreshold} onChange={...}>Show threshold</Checkbox>
  }
  // Add threshold line
  series={[
    ...dataSeries,
    {
      title: 'Threshold',
      type: 'threshold',
      y: 80,
    }
  ]}
/>
```

### Responsive Sizing

```jsx
<LineChart
  series={series}
  height={300}  // Fixed height
  // Width is dynamic, fills container
/>
```

## Data Visualization Colors

Use the categorical palette for data series:

```jsx
// Status colors
const statusColors = {
  critical: '#d13212',  // Red
  high: '#d13212',
  medium: '#f89256',    // Orange
  low: '#f2cd54',       // Yellow
  positive: '#1d8102',  // Green
  info: '#0972d3',      // Blue
  neutral: '#879596',   // Grey
};

// Categorical palette (use in order)
const categoricalColors = [
  '#688ae8', // Blue
  '#c33d69', // Pink
  '#2ea597', // Teal
  '#8456ce', // Purple
  '#e07941', // Orange
  // ... up to 50 colors
];
```

### Best Practices

- Use first N colors from categorical palette in order
- Max 8 series for line/bar charts
- Max 5 slices for pie/donut charts
- Use status colors for severity/state indicators
- Ensure 3:1 contrast minimum

## Accessibility

```jsx
<LineChart
  ariaLabel="Monthly revenue chart"
  ariaDescription="Line chart showing monthly revenue trends for the past year"
  i18nStrings={{
    legendAriaLabel: 'Legend',
    chartAriaRoleDescription: 'line chart',
    xAxisAriaRoleDescription: 'x axis',
    yAxisAriaRoleDescription: 'y axis',
  }}
/>
```

## Dashboard Pattern

```jsx
<Grid gridDefinition={[
  { colspan: 8 }, { colspan: 4 },
  { colspan: 4 }, { colspan: 4 }, { colspan: 4 }
]}>
  <Container header={<Header variant="h2">Trends</Header>}>
    <LineChart series={trendData} height={250} />
  </Container>

  <Container header={<Header variant="h2">Distribution</Header>}>
    <PieChart data={distributionData} variant="donut" />
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
```

## Best Practices

1. Choose chart type based on data relationship (trends → line, comparisons → bar)
2. Limit series to 8 for readability
3. Include axis titles with units
4. Provide loading, empty, and error states
5. Use consistent colors across related charts
6. Include aria labels for accessibility
7. Don't use charts for decoration
8. Start Y-axis at 0 unless showing relative changes
