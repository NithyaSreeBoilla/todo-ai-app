# Backend Instructions for FastAPI Todo App

## Overview
This backend uses FastAPI with async SQLAlchemy, following a clean architecture pattern with repository and service layers.

## Architecture

### Repository Pattern
- **Purpose**: Handle all database operations
- **No Business Logic**: Only CRUD operations, no validation or processing
- **Async Operations**: All methods must be async
- **Return Types**: Return model instances or lists, not schemas

```python
class TodoRepository:
    def __init__(self, session: AsyncSession):
        self.session = session

    async def get_all(self) -> List[Todo]:
        result = await self.session.execute(select(Todo))
        return result.scalars().all()

    async def create(self, todo_data: dict) -> Todo:
        todo = Todo(**todo_data)
        self.session.add(todo)
        await self.session.commit()
        await self.session.refresh(todo)
        return todo
```

### Service Layer
- **Purpose**: Business logic and validation
- **Dependencies**: Inject repositories via constructor
- **Validation**: Use Pydantic schemas for input validation
- **Error Handling**: Raise custom exceptions for business logic errors

```python
class TodoService:
    def __init__(self, repository: TodoRepository):
        self.repository = repository

    async def create_todo(self, todo_create: TodoCreate) -> TodoResponse:
        # Business logic here
        if len(todo_create.title) < 3:
            raise ValueError("Title must be at least 3 characters")

        todo = await self.repository.create(todo_create.dict())
        return TodoResponse.from_orm(todo)
```

### Routes
- **Purpose**: HTTP request/response handling only
- **No DB Access**: Never access database directly
- **Dependency Injection**: Use FastAPI's Depends for services
- **Response Models**: Return Pydantic response schemas

```python
@router.post("/", response_model=TodoResponse)
async def create_todo(
    todo_create: TodoCreate,
    service: TodoService = Depends(get_todo_service)
):
    return await service.create_todo(todo_create)
```

## Database Setup

### Async SQLAlchemy
- **Engine**: Create async engine with proper configuration
- **Session**: Use AsyncSession for all operations
- **Models**: Define with proper relationships and constraints

```python
from sqlalchemy.ext.asyncio import AsyncSession, create_async_engine
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "sqlite+aiosqlite:///./todo.db"

engine = create_async_engine(DATABASE_URL, echo=True)
async_session = sessionmaker(engine, class_=AsyncSession, expire_on_commit=False)

async def get_db() -> AsyncSession:
    async with async_session() as session:
        try:
            yield session
        finally:
            await session.close()
```

### Session Usage
- **Always Async**: Use `await session.execute()` for queries
- **Commit**: Always commit after modifications
- **Refresh**: Refresh objects after commit to get generated IDs
- **Context Managers**: Use async context managers for session management

## Dependency Injection

### Service Dependencies
- **Repository Injection**: Services receive repositories via constructor
- **Session Injection**: Repositories receive AsyncSession via constructor
- **FastAPI Depends**: Use Depends to inject services into routes

```python
def get_todo_repository(session: AsyncSession = Depends(get_db)) -> TodoRepository:
    return TodoRepository(session)

def get_todo_service(repository: TodoRepository = Depends(get_todo_repository)) -> TodoService:
    return TodoService(repository)
```

## Pydantic Schemas

### Request/Response Models
- **Separation**: Different schemas for create, update, response
- **Validation**: Use field validators for custom validation
- **ORM Mode**: Enable from_orm for easy conversion

```python
from pydantic import BaseModel, validator

class TodoBase(BaseModel):
    title: str
    description: Optional[str] = None
    completed: bool = False

class TodoCreate(TodoBase):
    @validator('title')
    def title_must_not_be_empty(cls, v):
        if not v.strip():
            raise ValueError('Title cannot be empty')
        return v

class TodoResponse(TodoBase):
    id: int
    created_at: datetime

    class Config:
        orm_mode = True
```

## Testing with pytest

### Test Structure
- **Async Tests**: Use pytest-asyncio for async test functions
- **Fixtures**: Create fixtures for database sessions and services
- **Mocking**: Mock external dependencies, not database for integration tests

```python
import pytest
from sqlalchemy.ext.asyncio import AsyncSession

@pytest.fixture
async def db_session():
    # Setup test database
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)

    async with AsyncSession(engine) as session:
        yield session

    # Cleanup
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.drop_all)

@pytest.mark.asyncio
async def test_create_todo(db_session: AsyncSession):
    repository = TodoRepository(db_session)
    service = TodoService(repository)

    todo_data = TodoCreate(title="Test Todo")
    result = await service.create_todo(todo_data)

    assert result.title == "Test Todo"
    assert result.id is not None
```

## Strict Rules

### Database Access
- **No DB in Routes**: Routes must never access database directly
- **Repository Only**: All DB operations through repository layer
- **AsyncSession**: Always use AsyncSession, never sync session

### Business Logic
- **No Logic in Repository**: Repositories contain only CRUD operations
- **Service Layer**: All business logic, validation, and processing in services
- **Custom Exceptions**: Raise specific exceptions for business errors

### Code Quality
- **Type Hints**: All functions, parameters, and return types must have type hints
- **Docstrings**: Google-style docstrings for all public methods
- **Naming**: snake_case for functions/variables, PascalCase for classes

### Error Handling
- **HTTPException**: Use FastAPI's HTTPException for API errors
- **Custom Exceptions**: Define business logic exceptions in services
- **Logging**: Log errors and important operations

### Dependency Injection
- **Constructor Injection**: Services and repositories use constructor injection
- **Depends**: Use FastAPI's Depends for route dependencies
- **No Global State**: Avoid global variables and singletons