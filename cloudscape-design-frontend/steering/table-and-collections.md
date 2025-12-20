# Cloudscape Table and Collections

Tables, cards, filtering, pagination, and collection management.

## TypeScript Types

Common types for table components:

```tsx
import { TableProps, CollectionPreferencesProps } from '@cloudscape-design/components';

// Item interface for your data
interface Resource {
  id: string;
  name: string;
  status: 'success' | 'error' | 'warning' | 'info' | 'pending';
  statusText: string;
  type: string;
  createdAt: string;
}

// Column definition type
type ColumnDefinition = TableProps.ColumnDefinition<Resource>;

// Selection type
type SelectionChangeDetail = TableProps.SelectionChangeDetail<Resource>;

// Preferences type
interface TablePreferences {
  pageSize: number;
  visibleContent: readonly string[];
  wrapLines: boolean;
  stripedRows: boolean;
}
```

## Basic Table

```tsx
import { useState } from 'react';
import { Table, Header, StatusIndicator, Box, TableProps } from '@cloudscape-design/components';

interface Resource {
  id: string;
  name: string;
  status: 'success' | 'error' | 'warning' | 'info';
  statusText: string;
}

const columnDefinitions: TableProps.ColumnDefinition<Resource>[] = [
  { id: 'id', header: 'ID', cell: item => item.id, sortingField: 'id' },
  { id: 'name', header: 'Name', cell: item => item.name, sortingField: 'name' },
  { id: 'status', header: 'Status', cell: item => (
    <StatusIndicator type={item.status}>{item.statusText}</StatusIndicator>
  )},
];

<Table<Resource>
  columnDefinitions={columnDefinitions}
  items={items}
  header={<Header counter={`(${items.length})`}>Resources</Header>}
  empty={<Box textAlign="center">No resources</Box>}
/>
```

## Table Variants

| Variant | Use Case |
|---------|----------|
| `container` | Standalone table with borders/shadows |
| `borderless` | Inside another container (dashboard, modal) |
| `full-page` | Full-page table view pattern |

```jsx
// Full-page table
<AppLayout
  contentType="table"
  content={
    <Table
      variant="full-page"
      stickyHeader
      header={<Header variant="awsui-h1-sticky">Resources</Header>}
      // ...
    />
  }
/>
```

## Selection

```tsx
import { useState } from 'react';
import { Table, Header, TableProps } from '@cloudscape-design/components';

interface Resource {
  id: string;
  name: string;
}

const [selectedItems, setSelectedItems] = useState<Resource[]>([]);

<Table<Resource>
  selectionType="multi"  // or "single"
  selectedItems={selectedItems}
  onSelectionChange={({ detail }) => setSelectedItems(detail.selectedItems)}
  ariaLabels={{
    selectionGroupLabel: 'Item selection',
    allItemsSelectionLabel: 'Select all',
    itemSelectionLabel: (data, row) => `Select ${row.name}`
  }}
  header={
    <Header counter={selectedItems.length ? `(${selectedItems.length}/${items.length})` : `(${items.length})`}>
      Resources
    </Header>
  }
/>
```

## Sorting

```jsx
const [sortingColumn, setSortingColumn] = useState({ sortingField: 'name' });
const [sortingDescending, setSortingDescending] = useState(false);

<Table
  sortingColumn={sortingColumn}
  sortingDescending={sortingDescending}
  onSortingChange={({ detail }) => {
    setSortingColumn(detail.sortingColumn);
    setSortingDescending(detail.sortingDescending);
  }}
  columnDefinitions={[
    { id: 'name', header: 'Name', cell: item => item.name, sortingField: 'name' },
    { id: 'date', header: 'Created', cell: item => item.date, sortingField: 'date' },
  ]}
/>
```

## Filtering

### TextFilter

```jsx
import { TextFilter } from '@cloudscape-design/components';

const [filterText, setFilterText] = useState('');

<Table
  filter={
    <TextFilter
      filteringText={filterText}
      filteringPlaceholder="Find resources"
      onChange={({ detail }) => setFilterText(detail.filteringText)}
    />
  }
  items={items.filter(item =>
    item.name.toLowerCase().includes(filterText.toLowerCase())
  )}
/>
```

### PropertyFilter

For complex multi-property filtering:

```jsx
import { PropertyFilter } from '@cloudscape-design/components';

const [query, setQuery] = useState({ tokens: [], operation: 'and' });

<Table
  filter={
    <PropertyFilter
      query={query}
      onChange={({ detail }) => setQuery(detail)}
      filteringOptions={[
        {
          propertyKey: 'status',
          operators: ['=', '!='],
          value: 'Status',
          values: [
            { value: 'active', label: 'Active' },
            { value: 'stopped', label: 'Stopped' },
          ]
        },
        {
          propertyKey: 'type',
          operators: ['=', '!='],
          value: 'Type',
          values: [
            { value: 't2.micro', label: 't2.micro' },
            { value: 't2.small', label: 't2.small' },
          ]
        },
      ]}
      i18nStrings={{
        filteringAriaLabel: 'Filter resources',
        filteringPlaceholder: 'Find resources',
        groupValuesText: 'Values',
        groupPropertiesText: 'Properties',
        operatorsText: 'Operators',
        operationAndText: 'and',
        operationOrText: 'or',
        clearFiltersText: 'Clear filters',
        removeTokenButtonAriaLabel: token => `Remove ${token.propertyKey}`,
        enteredTextLabel: text => `Use: "${text}"`
      }}
    />
  }
/>
```

## Pagination

```jsx
import { Pagination } from '@cloudscape-design/components';

const [currentPage, setCurrentPage] = useState(1);
const pageSize = 10;

const paginatedItems = items.slice((currentPage - 1) * pageSize, currentPage * pageSize);

<Table
  items={paginatedItems}
  pagination={
    <Pagination
      currentPageIndex={currentPage}
      pagesCount={Math.ceil(items.length / pageSize)}
      onChange={({ detail }) => setCurrentPage(detail.currentPageIndex)}
    />
  }
/>
```

## Collection Preferences

```jsx
import { CollectionPreferences } from '@cloudscape-design/components';

const [preferences, setPreferences] = useState({
  pageSize: 10,
  visibleContent: ['id', 'name', 'status'],
  wrapLines: false,
  stripedRows: false,
});

<Table
  preferences={
    <CollectionPreferences
      title="Preferences"
      confirmLabel="Confirm"
      cancelLabel="Cancel"
      preferences={preferences}
      onConfirm={({ detail }) => setPreferences(detail)}
      pageSizePreference={{
        title: 'Page size',
        options: [
          { value: 10, label: '10 items' },
          { value: 20, label: '20 items' },
          { value: 50, label: '50 items' },
        ]
      }}
      visibleContentPreference={{
        title: 'Visible columns',
        options: [
          { label: 'Properties', options: [
            { id: 'id', label: 'ID' },
            { id: 'name', label: 'Name' },
            { id: 'status', label: 'Status' },
          ]}
        ]
      }}
      wrapLinesPreference={{ label: 'Wrap lines', description: 'Wrap cell content' }}
      stripedRowsPreference={{ label: 'Striped rows', description: 'Alternate row colors' }}
    />
  }
/>
```

## useCollection Hook

Simplifies table state management:

```tsx
import { useState } from 'react';
import { Table, TextFilter, Pagination, Box, TableProps } from '@cloudscape-design/components';
import { useCollection } from '@cloudscape-design/collection-hooks';

interface Resource {
  id: string;
  name: string;
  status: string;
}

interface TableWithHooksProps {
  allItems: Resource[];
}

function TableWithHooks({ allItems }: TableWithHooksProps) {
  const columnDefinitions: TableProps.ColumnDefinition<Resource>[] = [
    { id: 'name', header: 'Name', cell: item => item.name, sortingField: 'name' },
    { id: 'status', header: 'Status', cell: item => item.status },
  ];

  const { items, filterProps, paginationProps, collectionProps } = useCollection(allItems, {
    filtering: {
      empty: <Box textAlign="center">No resources</Box>,
      noMatch: <Box textAlign="center">No matches</Box>,
    },
    pagination: { pageSize: 10 },
    sorting: {},
    selection: {},
  });

  return (
    <>
      <TextFilter {...filterProps} />
      <Table<Resource>
        {...collectionProps}
        items={items}
        columnDefinitions={columnDefinitions}
      />
      <Pagination {...paginationProps} />
    </>
  );
}
```

## Cards

Alternative to tables for visual collections:

```jsx
import { Cards, Header, Box } from '@cloudscape-design/components';

<Cards
  cardDefinition={{
    header: item => item.name,
    sections: [
      { id: 'status', header: 'Status', content: item => item.status },
      { id: 'type', header: 'Type', content: item => item.type },
      { id: 'description', header: 'Description', content: item => item.description },
    ]
  }}
  items={items}
  cardsPerRow={[{ cards: 1 }, { minWidth: 500, cards: 2 }, { minWidth: 800, cards: 3 }]}
  header={<Header counter={`(${items.length})`}>Resources</Header>}
  empty={<Box textAlign="center">No resources</Box>}
/>
```

### Cards with Selection

```jsx
<Cards
  selectionType="multi"
  selectedItems={selectedItems}
  onSelectionChange={({ detail }) => setSelectedItems(detail.selectedItems)}
  entireCardClickable
  // ...
/>
```

## Complete Table Example

```tsx
import { useState } from 'react';
import {
  Table, Header, Button, SpaceBetween, Box,
  TextFilter, Pagination, CollectionPreferences,
  StatusIndicator, TableProps, CollectionPreferencesProps
} from '@cloudscape-design/components';
import { useCollection } from '@cloudscape-design/collection-hooks';

interface Resource {
  id: string;
  name: string;
  status: string;
  statusType: 'success' | 'error' | 'warning' | 'info';
  type: string;
}

interface TablePreferences {
  pageSize: number;
  visibleContent?: readonly string[];
}

interface ResourceTableProps {
  resources: Resource[];
}

function ResourceTable({ resources }: ResourceTableProps) {
  const [selectedItems, setSelectedItems] = useState<Resource[]>([]);
  const [preferences, setPreferences] = useState<TablePreferences>({ pageSize: 10 });

  const columnDefinitions: TableProps.ColumnDefinition<Resource>[] = [
    { id: 'name', header: 'Name', cell: item => item.name, sortingField: 'name' },
    { id: 'status', header: 'Status', cell: item => (
      <StatusIndicator type={item.statusType}>{item.status}</StatusIndicator>
    )},
    { id: 'type', header: 'Type', cell: item => item.type },
  ];

  const { items, filterProps, paginationProps, collectionProps } = useCollection(
    resources,
    {
      filtering: { empty: 'No resources', noMatch: 'No matches' },
      pagination: { pageSize: preferences.pageSize },
      sorting: { defaultState: { sortingColumn: { sortingField: 'name' } } },
      selection: {},
    }
  );

  return (
    <Table<Resource>
      {...collectionProps}
      selectedItems={selectedItems}
      onSelectionChange={({ detail }) => setSelectedItems(detail.selectedItems)}
      selectionType="multi"
      columnDefinitions={columnDefinitions}
      items={items}
      header={
        <Header
          counter={selectedItems.length ? `(${selectedItems.length}/${resources.length})` : `(${resources.length})`}
          actions={
            <SpaceBetween direction="horizontal" size="xs">
              <Button disabled={!selectedItems.length}>Delete</Button>
              <Button variant="primary">Create</Button>
            </SpaceBetween>
          }
        >
          Resources
        </Header>
      }
      filter={<TextFilter {...filterProps} filteringPlaceholder="Find resources" />}
      pagination={<Pagination {...paginationProps} />}
      preferences={
        <CollectionPreferences
          preferences={preferences}
          onConfirm={({ detail }) => setPreferences(detail as TablePreferences)}
          pageSizePreference={{
            title: 'Page size',
            options: [
              { value: 10, label: '10' },
              { value: 20, label: '20' },
            ]
          }}
        />
      }
      empty={<Box textAlign="center">No resources to display</Box>}
    />
  );
}
```

## Best Practices

1. Always show item count in header: `(150)` or `(5/150)` when selected
2. Use `-` (hyphen) for empty cell values
3. First column should be unique identifier (name, ID)
4. Second column for status when relevant
5. Enable sorting/filtering/pagination for 5+ items
6. Use `full-page` variant with `stickyHeader` for main resource tables
7. Reset selection on filter/sort/page changes
8. Store preferences and restore on return
9. Use `useCollection` hook for complex tables
