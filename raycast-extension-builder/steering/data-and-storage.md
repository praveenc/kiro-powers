# Data and Storage

LocalStorage, preferences, API calls, and data fetching patterns.

## LocalStorage

Persist data locally between sessions.

### Basic Operations

```tsx
import { LocalStorage } from "@raycast/api";

// Store a value
await LocalStorage.setItem("key", "value");

// Retrieve a value
const value = await LocalStorage.getItem<string>("key");

// Remove a value
await LocalStorage.removeItem("key");

// Clear all storage
await LocalStorage.clear();

// Get all items
const allItems = await LocalStorage.allItems();
```

### Storing Objects

```tsx
interface Task {
  id: string;
  title: string;
  completed: boolean;
}

// Store array of objects
const tasks: Task[] = [
  { id: "1", title: "Buy milk", completed: false },
  { id: "2", title: "Call mom", completed: true },
];
await LocalStorage.setItem("tasks", JSON.stringify(tasks));

// Retrieve and parse
const stored = await LocalStorage.getItem<string>("tasks");
const tasks = stored ? JSON.parse(stored) : [];
```

### Custom Hook for LocalStorage

```tsx
import { LocalStorage } from "@raycast/api";
import { useState, useEffect } from "react";

function useLocalStorage<T>(key: string, initialValue: T) {
  const [value, setValue] = useState<T>(initialValue);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    LocalStorage.getItem<string>(key).then((stored) => {
      if (stored) {
        setValue(JSON.parse(stored));
      }
      setIsLoading(false);
    });
  }, [key]);

  const setStoredValue = async (newValue: T) => {
    setValue(newValue);
    await LocalStorage.setItem(key, JSON.stringify(newValue));
  };

  return [value, setStoredValue, isLoading] as const;
}

// Usage
const [tasks, setTasks, isLoading] = useLocalStorage<Task[]>("tasks", []);
```

## Preferences

Read user-configured preferences.

```tsx
import { getPreferenceValues } from "@raycast/api";

interface Preferences {
  apiKey: string;
  baseUrl: string;
  showCompleted: boolean;
}

export default function Command() {
  const { apiKey, baseUrl, showCompleted } = getPreferenceValues<Preferences>();

  // Use preferences
  const client = new ApiClient(baseUrl, apiKey);
}
```

### Required vs Optional

```tsx
interface Preferences {
  apiKey: string;        // required: true in manifest
  theme?: string;        // required: false in manifest
}

const prefs = getPreferenceValues<Preferences>();
const theme = prefs.theme ?? "default"; // Handle optional
```


## Fetching Data

### Basic Fetch

```tsx
import { List, showToast, Toast } from "@raycast/api";
import { useState, useEffect } from "react";

interface Item {
  id: string;
  title: string;
}

export default function Command() {
  const [items, setItems] = useState<Item[]>([]);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    async function fetchItems() {
      try {
        const response = await fetch("https://api.example.com/items");
        if (!response.ok) throw new Error("Failed to fetch");
        const data = await response.json();
        setItems(data);
      } catch (error) {
        showToast({
          style: Toast.Style.Failure,
          title: "Failed to load items",
          message: String(error),
        });
      } finally {
        setIsLoading(false);
      }
    }
    fetchItems();
  }, []);

  return (
    <List isLoading={isLoading}>
      {items.map((item) => (
        <List.Item key={item.id} title={item.title} />
      ))}
    </List>
  );
}
```

### With Authentication

```tsx
import { getPreferenceValues } from "@raycast/api";

interface Preferences {
  apiToken: string;
}

async function fetchWithAuth(endpoint: string) {
  const { apiToken } = getPreferenceValues<Preferences>();

  const response = await fetch(`https://api.example.com${endpoint}`, {
    headers: {
      Authorization: `Bearer ${apiToken}`,
      "Content-Type": "application/json",
    },
  });

  if (!response.ok) {
    throw new Error(`API error: ${response.status}`);
  }

  return response.json();
}
```

### POST Request

```tsx
async function createItem(title: string) {
  const { apiToken } = getPreferenceValues<Preferences>();

  const response = await fetch("https://api.example.com/items", {
    method: "POST",
    headers: {
      Authorization: `Bearer ${apiToken}`,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ title }),
  });

  if (!response.ok) {
    throw new Error("Failed to create item");
  }

  return response.json();
}
```

## @raycast/utils Hooks

Install: `npm install @raycast/utils`

### useFetch

```tsx
import { List } from "@raycast/api";
import { useFetch } from "@raycast/utils";

interface Item {
  id: string;
  title: string;
}

export default function Command() {
  const { data, isLoading, error } = useFetch<Item[]>(
    "https://api.example.com/items"
  );

  return (
    <List isLoading={isLoading}>
      {data?.map((item) => (
        <List.Item key={item.id} title={item.title} />
      ))}
    </List>
  );
}
```

### useFetch with Options

```tsx
const { data, isLoading, revalidate } = useFetch<Item[]>(
  "https://api.example.com/items",
  {
    headers: {
      Authorization: `Bearer ${apiToken}`,
    },
    keepPreviousData: true,
    initialData: [],
    execute: !!apiToken, // Only fetch if token exists
  }
);

// Refresh data
<Action title="Refresh" onAction={() => revalidate()} />
```

### usePromise

For non-fetch async operations:

```tsx
import { usePromise } from "@raycast/utils";

async function loadTasks(): Promise<Task[]> {
  const stored = await LocalStorage.getItem<string>("tasks");
  return stored ? JSON.parse(stored) : [];
}

export default function Command() {
  const { data: tasks, isLoading, revalidate } = usePromise(loadTasks);

  return (
    <List isLoading={isLoading}>
      {tasks?.map((task) => (
        <List.Item key={task.id} title={task.title} />
      ))}
    </List>
  );
}
```

### usePromise with Arguments

```tsx
async function searchItems(query: string): Promise<Item[]> {
  const response = await fetch(`https://api.example.com/search?q=${query}`);
  return response.json();
}

export default function Command() {
  const [searchText, setSearchText] = useState("");

  const { data, isLoading } = usePromise(searchItems, [searchText], {
    execute: searchText.length > 0,
  });

  return (
    <List
      isLoading={isLoading}
      onSearchTextChange={setSearchText}
      throttle
    >
      {data?.map((item) => (
        <List.Item key={item.id} title={item.title} />
      ))}
    </List>
  );
}
```

## Caching

### Simple Cache

```tsx
import { Cache } from "@raycast/api";

const cache = new Cache();

async function getCachedData<T>(key: string, fetcher: () => Promise<T>): Promise<T> {
  const cached = cache.get(key);
  if (cached) {
    return JSON.parse(cached);
  }

  const data = await fetcher();
  cache.set(key, JSON.stringify(data));
  return data;
}
```

### Cache with Expiry

```tsx
interface CacheEntry<T> {
  data: T;
  timestamp: number;
}

const CACHE_TTL = 5 * 60 * 1000; // 5 minutes

async function getCachedData<T>(key: string, fetcher: () => Promise<T>): Promise<T> {
  const cached = cache.get(key);

  if (cached) {
    const entry: CacheEntry<T> = JSON.parse(cached);
    if (Date.now() - entry.timestamp < CACHE_TTL) {
      return entry.data;
    }
  }

  const data = await fetcher();
  cache.set(key, JSON.stringify({ data, timestamp: Date.now() }));
  return data;
}
```

## Environment

```tsx
import { environment } from "@raycast/api";

// Check if development mode
if (environment.isDevelopment) {
  console.log("Running in development");
}

// Extension paths
console.log(environment.assetsPath);      // Path to assets folder
console.log(environment.supportPath);     // Path for extension data
console.log(environment.extensionName);   // Extension name
```

## Best Practices

1. Always show loading state while fetching
2. Handle errors gracefully with toasts
3. Use `@raycast/utils` hooks for cleaner code
4. Cache expensive API calls
5. Store sensitive data in preferences (type: password)
6. Use `keepPreviousData` to avoid flickering
7. Throttle search requests with `throttle` prop
