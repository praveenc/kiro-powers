# AI Extensions

Building AI-powered extensions with tools, instructions, and evals.

## What is an AI Extension?

An AI Extension is a regular extension with **tools** - functions that Raycast AI can call. Users interact via:
- "Ask [Extension Name]" in root search
- `@mention` in AI Chat or Quick AI

## Creating Tools

Tools live in `src/tools/` directory.

### Basic Tool

```tsx
// src/tools/greet.ts
export default function tool() {
  return "Hello, world!";
}
```

### Tool with Input

```tsx
// src/tools/greet.ts
type Input = {
  /**
   * The name of the person to greet
   */
  name: string;
};

/**
 * Greet a person by name
 */
export default function tool(input: Input) {
  return `Hello, ${input.name}!`;
}
```

### Tool with Complex Input

```tsx
// src/tools/create-task.ts
type Input = {
  /**
   * The title of the task
   */
  title: string;
  /**
   * Optional description of the task
   */
  description?: string;
  /**
   * Priority level: "low", "medium", or "high"
   */
  priority?: "low" | "medium" | "high";
  /**
   * Due date in ISO 8601 format (YYYY-MM-DD)
   */
  dueDate?: string;
};

/**
 * Create a new task in the task list
 */
export default async function tool(input: Input) {
  const task = await createTask(input);
  return {
    id: task.id,
    title: task.title,
    created: true,
  };
}
```

### Async Tool with API Call

```tsx
// src/tools/get-weather.ts
type Input = {
  /**
   * City name to get weather for
   */
  city: string;
};

/**
 * Get current weather for a city
 */
export default async function tool(input: Input) {
  const response = await fetch(
    `https://api.weather.com/current?city=${input.city}`
  );
  const data = await response.json();

  return {
    city: input.city,
    temperature: data.temp,
    conditions: data.conditions,
  };
}
```


## Tool Confirmations

Keep humans in the loop for important actions:

```tsx
// src/tools/delete-item.ts
import { Tool } from "@raycast/api";

type Input = {
  /**
   * ID of the item to delete
   */
  id: string;
};

export const confirmation: Tool.Confirmation<Input> = async (input) => {
  const item = await getItem(input.id);
  return {
    message: `Are you sure you want to delete "${item.name}"?`,
    info: [
      { name: "Item ID", value: input.id },
      { name: "Created", value: item.createdAt },
    ],
  };
};

/**
 * Delete an item permanently
 */
export default async function tool(input: Input) {
  await deleteItem(input.id);
  return { deleted: true, id: input.id };
}
```

### Dynamic Confirmation

```tsx
export const confirmation: Tool.Confirmation<Input> = async (input) => {
  // Only confirm for important items
  const item = await getItem(input.id);
  if (!item.isImportant) {
    return false; // Skip confirmation
  }

  return {
    message: `Delete important item "${item.name}"?`,
  };
};
```

## Manifest Configuration

### Adding Tools to package.json

```json
{
  "name": "my-ai-extension",
  "title": "My AI Extension",
  "tools": [
    {
      "name": "greet",
      "title": "Greet Person",
      "description": "Greet a person by their name"
    },
    {
      "name": "create-task",
      "title": "Create Task",
      "description": "Create a new task with title, description, and due date"
    },
    {
      "name": "get-todos",
      "title": "Get Todos",
      "description": "Retrieve all todos from the list"
    }
  ]
}
```

Tool `name` maps to `src/tools/{name}.ts`.

## AI Instructions

Provide context to help AI use your extension:

### In package.json

```json
{
  "ai": {
    "instructions": "When creating tasks, always ask for a due date if not provided. Format dates as YYYY-MM-DD."
  }
}
```

### In ai.json (for longer instructions)

```json
{
  "instructions": "This extension manages a todo list. Tasks have titles, descriptions, priorities (low/medium/high), and due dates. When listing tasks, summarize by priority. When creating tasks without a priority, default to medium."
}
```

### In ai.yaml (most readable for long text)

```yaml
instructions: |
  This extension manages a todo list.

  Task properties:
  - title: required
  - description: optional
  - priority: low, medium, or high (default: medium)
  - dueDate: ISO 8601 format (YYYY-MM-DD)

  Guidelines:
  - Always confirm before deleting tasks
  - Summarize tasks by priority when listing
  - Ask for due date if not provided
```

## Evals

Test your AI extension with evals - integration tests for AI behavior.

### Basic Eval

```json
{
  "ai": {
    "evals": [
      {
        "input": "@my-extension what are my todos?",
        "mocks": {
          "get-todos": [
            { "id": "1", "title": "Buy milk", "isCompleted": false },
            { "id": "2", "title": "Call mom", "isCompleted": true }
          ]
        },
        "expected": [
          { "callsTool": "get-todos" },
          { "includes": "Buy milk" }
        ]
      }
    ]
  }
}
```

### Eval Expectations

```json
{
  "expected": [
    // Check tool was called
    { "callsTool": "get-todos" },

    // Check tool arguments
    {
      "callsTool": {
        "name": "create-task",
        "arguments": {
          "title": { "includes": "meeting" },
          "priority": "high"
        }
      }
    },

    // Check response content
    { "includes": "created" },
    { "matches": "\\d+ tasks?" },

    // AI-validated criteria
    { "meetsCriteria": "Confirms the task was created successfully" },

    // Negative check
    { "not": { "callsTool": "delete-task" } }
  ]
}
```

### Running Evals

```bash
npx ray evals
```

## Best Practices

1. **Descriptive JSDoc** - AI uses comments to understand tools
2. **Type hints in descriptions** - Mention formats like "ISO 8601"
3. **Confirmations for destructive actions** - Delete, update, send
4. **Return structured data** - Objects are easier for AI to interpret
5. **Write evals for common use cases** - They also serve as example prompts
6. **Keep instructions focused** - Don't conflict with other extensions
7. **Test with various phrasings** - Users ask in different ways
