# Copilot Instructions for Todo App

## Project Overview
This is a full-stack Todo application with FastAPI backend and React frontend.

## Backend Requirements (FastAPI + SQLAlchemy)

### Architecture
- **Strict Layer Separation**: Route → Service → Repository
- Routes: Handle HTTP requests/responses only
- Services: Business logic and validation
- Repositories: Database operations only

### Code Standards
- **Async Everywhere**: Use `async`/`await` for all database and external operations
- **Type Hints**: Mandatory on all functions, parameters, and return types
- **Docstrings**: Google-style docstrings for all public functions and classes
- **Clean Code**: Descriptive names, single responsibility, DRY principle

### Error Handling
- **Custom Exceptions**: Define specific exceptions for business logic errors
- **HTTP Status Codes**: Use appropriate codes (400, 404, 422, 500)
- **Validation**: Pydantic models for request/response validation
- **Logging**: Structured logging for errors and important events

### Database
- **SQLAlchemy**: Use async SQLAlchemy with proper session management
- **Migrations**: Alembic for database schema changes
- **Models**: Clear relationships, constraints, and indexes

## Frontend Requirements (React)

### Architecture
- **Component Structure**: Functional components with hooks
- **State Management**: React state or Context API (no external libraries unless necessary)
- **Separation**: UI components, custom hooks, utilities

### Code Standards
- **TypeScript**: Use TypeScript for type safety
- **Clean Code**: Descriptive component names, single responsibility
- **Error Handling**: Try-catch blocks, user-friendly error messages
- **Styling**: CSS modules or styled-components

## General Rules

### Naming Conventions
- **Python**: snake_case for variables/functions, PascalCase for classes
- **JavaScript/TypeScript**: camelCase for variables/functions, PascalCase for components/classes
- **Files**: Descriptive names matching content

### Testing
- **Backend**: pytest with async support, mock external dependencies
- **Frontend**: Jest and React Testing Library
- **Coverage**: Aim for 80%+ coverage

### Security
- **Input Validation**: Always validate and sanitize inputs
- **Authentication**: JWT or similar for user sessions
- **CORS**: Properly configured for frontend-backend communication

### Performance
- **Database**: Use indexes, avoid N+1 queries, pagination for lists
- **Frontend**: Lazy loading, memoization, efficient re-renders

## Enforcement
- **Never** mix layers (e.g., database code in routes)
- **Always** use type hints and docstrings in Python
- **Always** handle errors appropriately
- **Never** hardcode values; use configuration
- **Always** follow the specified architecture pattern

## File Structure
```
api/
├── app/
│   ├── routes/
│   ├── services/
│   ├── repositories/
│   ├── models/
│   └── core/
web/
├── src/
│   ├── components/
│   ├── hooks/
│   ├── services/
│   └── utils/
```