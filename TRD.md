# Technical Requirements Document (TRD)

## Overview
This document explains the technical structure of a simple Todo application for beginner developers.
The backend uses FastAPI and SQL Server with SQLAlchemy. The application follows a clean architecture: Route → Service → Repository.

## Architecture Flow
The Todo app is divided into three main layers:

1. **Routes**
   - Handle HTTP requests and responses.
   - Do not contain business logic or direct database access.
   - Use FastAPI decorators like `@router.get`, `@router.post`, etc.

2. **Services**
   - Contain business logic and validation.
   - Call repository methods to get or save data.
   - Convert data between domain models and response schemas.

3. **Repositories**
   - Handle all database operations using SQLAlchemy.
   - Work with `AsyncSession` to perform queries and updates.
   - Do not contain business rules or validation logic.

### Example Flow
- A client sends a `POST /todos` request.
- The route receives the request and validates the input with a Pydantic schema.
- The route calls a service method like `create_todo()`.
- The service applies rules and calls the repository to save the todo.
- The repository writes the todo to the SQL Server database.
- The service returns a response object.
- The route sends the response back to the client.

## Folder Structure
Keep the backend code organized in folders that match the architecture.

```
api/
├── app/
│   ├── routes/           # API route handlers
│   ├── services/         # Business logic layer
│   ├── repositories/     # Database access layer
│   ├── models/           # SQLAlchemy models
│   ├── schemas/          # Pydantic request/response schemas
│   └── core/             # Configuration and dependency code
```

### Folder Responsibilities
- `routes/`: Define endpoints and use FastAPI dependency injection.
- `services/`: Implement todo-specific business rules.
- `repositories/`: Use SQLAlchemy to talk to SQL Server.
- `models/`: Define the `Todo` table structure.
- `schemas/`: Define request and response JSON shapes.
- `core/`: Configure the database engine and session management.

## API Endpoints
The application supports the basic CRUD operations for Todo items.

### `GET /todos`
- Returns a list of all todos.
- No request body needed.
- Response: array of todo objects.

### `POST /todos`
- Creates a new todo.
- Request body includes:
  - `title` (required)
  - `description` (optional)
- Response: created todo object.

### `GET /todos/{id}`
- Returns a single todo by its ID.
- Response: todo object.

### `PUT /todos/{id}`
- Updates a todo item.
- Request body can include:
  - `title`
  - `description`
  - `completed`
- Response: updated todo object.

### `DELETE /todos/{id}`
- Deletes a todo item by ID.
- Response: confirmation or status message.

## Database Structure
The database stores only todo items. Each todo record has basic fields.

### Table: `todos`
| Column      | Type        | Description |
|-------------|-------------|-------------|
| `id`        | INT         | Primary key, unique identifier |
| `title`     | NVARCHAR(255) | Todo title, required |
| `description` | NVARCHAR(MAX) | Optional task details |
| `completed` | BIT         | True or false flag for completion |
| `created_at`| DATETIME2   | Timestamp when todo was created |
| `updated_at`| DATETIME2   | Timestamp when todo was last updated |

### Key Points
- Use `id` as the primary key.
- Keep `title` required and non-empty.
- Use `completed` to track if the task is done.
- Store creation and update timestamps for basic history.

## Beginner Tips
- Start by building the route handlers first.
- Then add the service layer to keep business rules separate.
- Finally, build the repository layer to interact with SQL Server.
- Use Pydantic schemas to validate incoming data and shape responses.
- Keep each function simple and focused on one task.

## Summary
This TRD gives a beginner-friendly picture of how the Todo app is built:
- A clean Route → Service → Repository flow.
- A clear folder structure for backend code.
- Simple API endpoints for todos.
- A straightforward SQL Server table design.

Follow these guidelines to keep the app easy to understand and maintain.