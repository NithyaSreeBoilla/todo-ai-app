# Todo AI App

A simple Todo application built with FastAPI backend, React frontend, and SQL Server database.

## Project Overview

This application allows users to create, read, update, and delete todo items. It features an AI-powered assistant to help manage tasks efficiently.

## Tech Stack

- **Backend**: FastAPI (Python)
- **Frontend**: React (JavaScript)
- **Database**: SQL Server
- **AI Integration**: [Specify AI service if applicable, e.g., OpenAI API]

## Setup Steps

### Prerequisites

- Python 3.8+
- Node.js 14+
- SQL Server (local or cloud instance)
- Git

### Backend Setup

1. Navigate to the `api` folder:
   ```
   cd api
   ```

2. Create a virtual environment:
   ```
   python -m venv venv
   ```

3. Activate the virtual environment:
   - Windows: `venv\Scripts\activate`
   - macOS/Linux: `source venv/bin/activate`

4. Install dependencies:
   ```
   pip install -r requirements.txt
   ```

5. Set up the database:
   - Create a SQL Server database named `todo_db`
   - Update the database connection string in `config.py` or environment variables

6. Run the backend:
   ```
   uvicorn main:app --reload
   ```

### Frontend Setup

1. Navigate to the `web` folder:
   ```
   cd web
   ```

2. Install dependencies:
   ```
   npm install
   ```

3. Start the development server:
   ```
   npm start
   ```

### Database Setup

1. Install SQL Server (if not already installed)
2. Create a new database named `todo_db`
3. Run the database migration scripts (if provided in the `api` folder)

## Folder Structure

```
todo-ai-app/
├── api/                 # FastAPI backend
│   ├── main.py          # Main application file
│   ├── models.py        # Database models
│   ├── routes.py        # API routes
│   └── requirements.txt # Python dependencies
├── web/                 # React frontend
│   ├── src/             # Source code
│   ├── public/          # Static assets
│   └── package.json     # Node.js dependencies
└── README.md            # This file
```

## Usage

1. Start the backend server (as described in setup)
2. Start the frontend server (as described in setup)
3. Open your browser to `http://localhost:3000`
4. Create, edit, and manage your todos!

## Contributing

Feel free to submit issues and pull requests.

## License

[Specify license if applicable]