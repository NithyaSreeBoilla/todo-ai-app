# Frontend Instructions for React Todo App

## Overview
This frontend uses React with TypeScript, React Query for data fetching, and Axios for API calls.

## Architecture

### Component Structure
- **Functional Components**: Use functional components with hooks
- **Small Components**: Keep components focused on single responsibility
- **Custom Hooks**: Extract reusable logic into custom hooks
- **Separation**: UI components, custom hooks, services, types

### File Organization
```
src/
├── components/     # UI components
├── hooks/         # Custom hooks
├── services/      # API services
├── types/         # TypeScript types
├── utils/         # Utility functions
└── pages/         # Page components
```

## TypeScript Usage

### Type Definitions
- **Interfaces**: Define interfaces for all data structures
- **API Types**: Separate types for API requests and responses
- **Component Props**: Type all component props

```typescript
// types/todo.ts
export interface Todo {
  id: number;
  title: string;
  description?: string;
  completed: boolean;
  created_at: string;
}

export interface TodoCreate {
  title: string;
  description?: string;
}

export interface TodoUpdate {
  title?: string;
  description?: string;
  completed?: boolean;
}
```

### Component Typing
- **Props**: Use interfaces for component props
- **State**: Type all useState hooks
- **Events**: Type event handlers properly

```typescript
interface TodoItemProps {
  todo: Todo;
  onToggle: (id: number) => void;
  onDelete: (id: number) => void;
}

const TodoItem: React.FC<TodoItemProps> = ({ todo, onToggle, onDelete }) => {
  return (
    <div>
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={() => onToggle(todo.id)}
      />
      <span>{todo.title}</span>
      <button onClick={() => onDelete(todo.id)}>Delete</button>
    </div>
  );
};
```

## React Query Setup

### Query Client
- **Global Provider**: Wrap app with QueryClientProvider
- **Configuration**: Set default options for queries and mutations

```typescript
// App.tsx
import { QueryClient, QueryClientProvider } from 'react-query';

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 1000 * 60 * 5, // 5 minutes
      cacheTime: 1000 * 60 * 10, // 10 minutes
    },
  },
});

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      {/* App content */}
    </QueryClientProvider>
  );
}
```

### Custom Hooks
- **useQuery**: For fetching data
- **useMutation**: For creating/updating/deleting
- **Query Keys**: Use consistent query keys

```typescript
// hooks/useTodos.ts
import { useQuery, useMutation, useQueryClient } from 'react-query';
import { getTodos, createTodo, updateTodo, deleteTodo } from '../services/todoService';

export const useTodos = () => {
  return useQuery(['todos'], getTodos);
};

export const useCreateTodo = () => {
  const queryClient = useQueryClient();

  return useMutation(createTodo, {
    onSuccess: () => {
      queryClient.invalidateQueries(['todos']);
    },
  });
};

export const useUpdateTodo = () => {
  const queryClient = useQueryClient();

  return useMutation(updateTodo, {
    onSuccess: () => {
      queryClient.invalidateQueries(['todos']);
    },
  });
};

export const useDeleteTodo = () => {
  const queryClient = useQueryClient();

  return useMutation(deleteTodo, {
    onSuccess: () => {
      queryClient.invalidateQueries(['todos']);
    },
  });
};
```

## Axios API Services

### Service Layer
- **Separation**: Keep API logic separate from components
- **Base URL**: Configure axios instance with base URL
- **Error Handling**: Handle errors consistently

```typescript
// services/api.ts
import axios from 'axios';

const api = axios.create({
  baseURL: process.env.REACT_APP_API_URL || 'http://localhost:8000',
  timeout: 10000,
});

api.interceptors.response.use(
  (response) => response,
  (error) => {
    console.error('API Error:', error);
    return Promise.reject(error);
  }
);

export default api;
```

### Todo Service
- **CRUD Operations**: Separate functions for each operation
- **Type Safety**: Use proper TypeScript types
- **Return Types**: Return promises with correct types

```typescript
// services/todoService.ts
import api from './api';
import { Todo, TodoCreate, TodoUpdate } from '../types/todo';

export const getTodos = async (): Promise<Todo[]> => {
  const response = await api.get('/todos');
  return response.data;
};

export const createTodo = async (todo: TodoCreate): Promise<Todo> => {
  const response = await api.post('/todos', todo);
  return response.data;
};

export const updateTodo = async (id: number, updates: TodoUpdate): Promise<Todo> => {
  const response = await api.put(`/todos/${id}`, updates);
  return response.data;
};

export const deleteTodo = async (id: number): Promise<void> => {
  await api.delete(`/todos/${id}`);
};
```

## Component Patterns

### Container/Presentational
- **Presentational**: Pure UI components, receive data via props
- **Container**: Handle data fetching and state, pass to presentational

```typescript
// components/TodoList.tsx (Presentational)
interface TodoListProps {
  todos: Todo[];
  onToggle: (id: number) => void;
  onDelete: (id: number) => void;
}

export const TodoList: React.FC<TodoListProps> = ({ todos, onToggle, onDelete }) => {
  return (
    <ul>
      {todos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={onToggle}
          onDelete={onDelete}
        />
      ))}
    </ul>
  );
};

// pages/TodosPage.tsx (Container)
export const TodosPage: React.FC = () => {
  const { data: todos, isLoading } = useTodos();
  const updateMutation = useUpdateTodo();
  const deleteMutation = useDeleteTodo();

  if (isLoading) return <div>Loading...</div>;

  return (
    <TodoList
      todos={todos || []}
      onToggle={(id) => updateMutation.mutate({ id, completed: true })}
      onDelete={(id) => deleteMutation.mutate(id)}
    />
  );
};
```

## Custom Hooks

### Form Handling
- **useState**: For form state
- **Validation**: Add form validation logic
- **Submission**: Handle form submission

```typescript
// hooks/useTodoForm.ts
import { useState } from 'react';
import { useCreateTodo } from './useTodos';
import { TodoCreate } from '../types/todo';

export const useTodoForm = () => {
  const [title, setTitle] = useState('');
  const [description, setDescription] = useState('');
  const createMutation = useCreateTodo();

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (!title.trim()) return;

    createMutation.mutate({
      title: title.trim(),
      description: description.trim() || undefined,
    });

    setTitle('');
    setDescription('');
  };

  return {
    title,
    setTitle,
    description,
    setDescription,
    handleSubmit,
    isLoading: createMutation.isLoading,
  };
};
```

## Strict Rules

### Component Design
- **Hooks Only**: Use functional components with hooks, no class components
- **Small Components**: Keep components under 50 lines, single responsibility
- **No Inline Functions**: Avoid inline arrow functions in render

### API Logic
- **Service Layer**: All API calls through service functions
- **No Direct Axios**: Never use axios directly in components
- **Consistent Error Handling**: Handle errors in services or hooks

### TypeScript
- **Strict Typing**: All variables, props, and return types must be typed
- **No Any**: Avoid using `any` type
- **Interface Usage**: Use interfaces over types for object shapes

### State Management
- **React Query**: Use React Query for server state
- **Local State**: Use useState for local component state
- **No External Libraries**: Avoid Redux/MobX unless absolutely necessary

### Performance
- **Memoization**: Use React.memo for expensive components
- **Query Keys**: Use descriptive query keys for cache management
- **Optimistic Updates**: Consider optimistic updates for better UX