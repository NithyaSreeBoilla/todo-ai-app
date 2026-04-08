# Product Requirements Document (PRD)

## Overview
The Todo application is a simple single-user task manager. It allows a single user to create, read, update, and delete todo items. The system is built with a FastAPI backend, a React frontend, and SQL Server as the database.

## Goals
- Provide a lightweight todo manager for a single user
- Keep the application simple and focused on task management
- Maintain clean API design and consistent data flow
- Deliver a responsive frontend experience

## Features
1. Create Todo
   - Add a new todo item with a title and optional description
   - Default status should be incomplete
2. Read Todos
   - Display a list of existing todos
   - Show status, title, description, and creation date
3. Update Todo
   - Edit title and description
   - Toggle completion status
4. Delete Todo
   - Remove a todo item permanently
5. Basic validation
   - Title is required
   - Title must not be empty

## API Requirements
### Endpoints
- `GET /todos`
  - Returns all todos for the single user
  - Response: list of todo objects

- `POST /todos`
  - Create a new todo item
  - Request body: title, optional description
  - Response: created todo object

- `GET /todos/{id}`
  - Retrieve a single todo item by ID
  - Response: todo object

- `PUT /todos/{id}`
  - Update an existing todo item
  - Request body: title, description, completed
  - Response: updated todo object

- `DELETE /todos/{id}`
  - Delete a todo item by ID
  - Response: status confirmation

### Validation & Error Handling
- Return `400 Bad Request` for invalid request bodies
- Return `404 Not Found` for missing todo IDs
- Use JSON error responses with `detail` text

### Response Schema
```json
{
  "id": 1,
  "title": "Buy groceries",
  "description": "Milk, eggs, bread",
  "completed": false,
  "created_at": "2026-04-08T10:00:00Z"
}
```

## Data Model
### Todo
- `id`: integer, primary key
- `title`: string, required
- `description`: string, optional
- `completed`: boolean, default `false`
- `created_at`: datetime, auto-generated
- `updated_at`: datetime, auto-updated

### SQL Server Table
- Table name: `todos`
- Columns:
  - `id` INT IDENTITY(1,1) PRIMARY KEY
  - `title` NVARCHAR(255) NOT NULL
  - `description` NVARCHAR(MAX) NULL
  - `completed` BIT NOT NULL DEFAULT 0
  - `created_at` DATETIME2 NOT NULL DEFAULT SYSUTCDATETIME()
  - `updated_at` DATETIME2 NOT NULL DEFAULT SYSUTCDATETIME()

## Basic User Flow
1. User opens the app
2. User sees the todo list
   - If no todos exist, show an empty state prompt
3. User adds a new todo
   - Enter title and optional description
   - Submit to create the todo
4. User reviews todos
   - Mark items complete/incomplete
   - Edit item details
   - Delete items if no longer needed
5. User actions update the UI immediately
   - Changes saved through API calls
   - List refreshes to reflect current data

## Non-Goals
- No authentication or authorization
- No multi-user support
- No categories, projects, or tags
- No advanced scheduling or reminders

## Success Criteria
- Users can manage todos with a minimal UI
- Backend supports all CRUD operations reliably
- API follows clear validation and error rules
- Data is persisted in SQL Server correctly
- Frontend feels responsive and straightforward
