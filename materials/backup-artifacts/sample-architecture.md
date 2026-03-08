# To-Do App - Technical Architecture

## Overview

This document describes the technical architecture for the To-Do App based on the requirements in [prd.md](./prd.md).

**Tech Stack:**
- React 18+ (UI framework)
- TypeScript (type safety)
- TailwindCSS (styling)
- Vite (build tool)
- Vitest (testing)

---

## System Architecture

### High-Level Design

```
┌────────────────────────────┐
│    Browser (Client)      │
│                            │
│  ┌────────────────────┐  │
│  │   React App         │  │
│  │   (Components)      │  │
│  └────────┬───────────┘  │
│            │               │
│  ┌────────┴───────────┐  │
│  │  React Context      │  │
│  │  (State)            │  │
│  └────────┬───────────┘  │
│            │               │
│  ┌────────┴───────────┐  │
│  │  Local Storage      │  │
│  │  (Persistence)      │  │
│  └────────────────────┘  │
└────────────────────────────┘
```

### Component Hierarchy

```
App
├── TodoProvider (Context)
    ├── Header
    ├── AddTodoForm
    └── TodoList
        ├── TodoItem
        ├── TodoItem
        └── TodoItem (repeated)
```

---

## Data Model

### Todo Interface

```typescript
interface Todo {
  id: string;           // Unique identifier (UUID)
  text: string;         // Task description
  completed: boolean;   // Completion status
  createdAt: number;    // Timestamp (milliseconds)
}
```

### Example Data

```json
[
  {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "text": "Buy groceries",
    "completed": false,
    "createdAt": 1709856000000
  },
  {
    "id": "6ba7b810-9dad-11d1-80b4-00c04fd430c8",
    "text": "Write workshop guide",
    "completed": true,
    "createdAt": 1709856120000
  }
]
```

---

## Component Specifications

### App Component
**File:** `src/App.tsx`

**Responsibilities:**
- Root component
- Wraps app in TodoProvider
- Renders Header, AddTodoForm, TodoList

**Props:** None

---

### TodoProvider (Context)
**File:** `src/context/TodoContext.tsx`

**Responsibilities:**
- Manage global todo state
- Provide actions: addTodo, toggleTodo, deleteTodo
- Handle local storage sync

**State:**
```typescript
interface TodoContextState {
  todos: Todo[];
  addTodo: (text: string) => void;
  toggleTodo: (id: string) => void;
  deleteTodo: (id: string) => void;
}
```

**Storage Key:** `bmad-todos`

---

### Header Component
**File:** `src/components/Header.tsx`

**Responsibilities:**
- Display app title
- Show task count ("5 tasks")

**Props:**
```typescript
interface HeaderProps {
  taskCount: number;
}
```

---

### AddTodoForm Component
**File:** `src/components/AddTodoForm.tsx`

**Responsibilities:**
- Render input field
- Render "Add" button
- Handle form submission
- Validate input (prevent empty tasks)
- Clear input after submission

**Props:**
```typescript
interface AddTodoFormProps {
  onAddTodo: (text: string) => void;
}
```

**State:**
```typescript
const [text, setText] = useState('');
```

---

### TodoList Component
**File:** `src/components/TodoList.tsx`

**Responsibilities:**
- Render list of TodoItem components
- Handle empty state ("No tasks yet")

**Props:**
```typescript
interface TodoListProps {
  todos: Todo[];
}
```

---

### TodoItem Component
**File:** `src/components/TodoItem.tsx`

**Responsibilities:**
- Render single task
- Show checkbox/complete button
- Show delete button
- Handle completion toggle
- Handle deletion

**Props:**
```typescript
interface TodoItemProps {
  todo: Todo;
  onToggle: (id: string) => void;
  onDelete: (id: string) => void;
}
```

**Visual States:**
- **Active:** Normal text, empty checkbox
- **Completed:** Strikethrough text, checked checkbox, muted color

---

## Data Flow

### Adding a Task

```
1. User types in AddTodoForm input
2. User clicks "Add" button
3. AddTodoForm.handleSubmit() calls onAddTodo(text)
4. TodoContext.addTodo() creates new Todo object
5. TodoContext updates state with new todo
6. TodoContext saves to localStorage
7. TodoList re-renders with new item
8. AddTodoForm clears input field
```

### Completing a Task

```
1. User clicks checkbox in TodoItem
2. TodoItem.handleToggle() calls onToggle(id)
3. TodoContext.toggleTodo() finds todo by id
4. TodoContext updates todo.completed = !todo.completed
5. TodoContext saves to localStorage
6. TodoItem re-renders with strikethrough style
```

### Deleting a Task

```
1. User clicks delete button in TodoItem
2. TodoItem.handleDelete() calls onDelete(id)
3. TodoContext.deleteTodo() filters out todo by id
4. TodoContext saves to localStorage
5. TodoList re-renders without deleted item
```

---

## State Management

### Why React Context?

**Pros:**
- No external dependencies
- Built into React
- Simple for small apps
- Easy to understand

**Cons:**
- Can cause unnecessary re-renders at scale
- Less developer tooling than Redux
- Not ideal for very large apps

**Decision:** For a small to-do app, Context is perfect. See [ADR 001](./adrs/001-react-context.md) for details.

---

## Styling Strategy

### TailwindCSS Utility Classes

**Example component:**
```tsx
<div className="flex items-center gap-2 p-4 border-b">
  <input type="checkbox" className="w-5 h-5" />
  <span className="flex-1 text-gray-800">Task text</span>
  <button className="text-red-500 hover:text-red-700">Delete</button>
</div>
```

**Design Tokens:**
- Primary color: Blue-500 (`#3B82F6`)
- Text color: Gray-800 (`#1F2937`)
- Muted text: Gray-500 (`#6B7280`)
- Border: Gray-200 (`#E5E7EB`)
- Error: Red-500 (`#EF4444`)

---

## Local Storage Implementation

### Storage Key
```typescript
const STORAGE_KEY = 'bmad-todos';
```

### Save Operation
```typescript
const saveTodos = (todos: Todo[]) => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
};
```

### Load Operation
```typescript
const loadTodos = (): Todo[] => {
  const stored = localStorage.getItem(STORAGE_KEY);
  return stored ? JSON.parse(stored) : [];
};
```

### When to Save
- After addTodo()
- After toggleTodo()
- After deleteTodo()

### When to Load
- On TodoProvider mount (useEffect)

---

## Error Handling

### Local Storage Errors

```typescript
try {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
} catch (error) {
  console.error('Failed to save todos:', error);
  // Gracefully degrade - app still works, just doesn't persist
}
```

**Common Issues:**
- Storage quota exceeded
- Private browsing mode
- Storage disabled by user

**Mitigation:** App continues to work in memory-only mode.

---

## Testing Strategy

### Unit Tests (Vitest)

**TodoContext:**
- Test addTodo adds item to state
- Test toggleTodo changes completed status
- Test deleteTodo removes item
- Test localStorage save/load

**AddTodoForm:**
- Test input value changes
- Test form submission calls onAddTodo
- Test empty input is prevented
- Test input clears after submission

**TodoItem:**
- Test checkbox click calls onToggle
- Test delete button calls onDelete
- Test completed styling applied

### Integration Tests

**Full Flow:**
1. Add task → verify appears in list
2. Complete task → verify strikethrough
3. Delete task → verify removed from list
4. Reload page → verify tasks persist

---

## Performance Considerations

### Optimization Strategies

1. **Memoization**
   ```typescript
   const todoListMemo = useMemo(() => 
     todos.map(todo => <TodoItem key={todo.id} todo={todo} />),
     [todos]
   );
   ```

2. **Lazy Loading**
   - Not needed for small lists (<100 items)
   - Consider if supporting 1000+ tasks

3. **Debounce Local Storage**
   - Not implemented in v1.0
   - Consider if users complain about performance

### Performance Targets

- **Initial load:** <1 second
- **Add task:** <100ms (perceived instantly)
- **Toggle task:** <50ms
- **Delete task:** <50ms

---

## Accessibility

### Keyboard Navigation

- Tab through input, add button, checkboxes, delete buttons
- Enter to submit form
- Space to toggle checkboxes
- Focus indicators visible

### ARIA Labels

```tsx
<input 
  type="checkbox" 
  aria-label="Mark task as complete"
  aria-checked={todo.completed}
/>

<button 
  aria-label="Delete task"
  onClick={handleDelete}
>
  🗑️
</button>
```

### Screen Reader Support

- Announce when tasks added
- Announce when tasks completed/uncompleted
- Announce when tasks deleted

---

## Security Considerations

### No Backend = No Server-Side Vulnerabilities

**Pros:**
- No SQL injection
- No XSS from server
- No CSRF tokens needed

**Local Storage:**
- Data stored in plaintext
- Accessible to any script on same origin
- Not suitable for sensitive data

**Mitigation:**
- Don't store sensitive information in tasks
- This is acceptable for a task list

---

## Deployment

### Build Process

```bash
npm run build
# Outputs to: dist/
```

### Deployment Options

1. **Static Hosting:**
   - Vercel (recommended)
   - Netlify
   - GitHub Pages

2. **CDN:**
   - Cloudflare Pages

3. **Self-Hosted:**
   - Any web server (nginx, Apache)

**No backend required** - just serve static files.

---

## Future Considerations (Out of Scope v1.0)

### Potential v1.1 Features

- **Export/Import:** Download tasks as JSON
- **Categories:** Tag tasks (work, personal, etc.)
- **Dark Mode:** User preference toggle

### Potential v2.0 Features

- **Cloud Sync:** Firebase or Supabase backend
- **Multi-User:** Authentication and collaboration
- **Mobile App:** React Native port

**Decision:** Keep v1.0 simple. Gather user feedback before adding complexity.

---

## Architecture Decision Records (ADRs)

See `docs/adrs/` folder for detailed decisions:

- [ADR 001: Use React Context for State Management](./adrs/001-react-context.md)
- [ADR 002: Use Local Storage for Persistence](./adrs/002-local-storage.md)
- [ADR 003: Use TailwindCSS for Styling](./adrs/003-tailwindcss.md)

---

## Questions & Answers

**Q: Why not Redux?**  
A: Overkill for this small app. Context is simpler and sufficient.

**Q: Why not a backend?**  
A: Not needed for v1.0. Keeps complexity low and deployment simple.

**Q: What about task limits?**  
A: LocalStorage limit is ~5-10MB. Enough for 10,000+ tasks.

**Q: Why TypeScript?**  
A: Type safety catches bugs early. Worth the small learning curve.

---

**Next Step:** Implement first user story using `@bmad /start-story`