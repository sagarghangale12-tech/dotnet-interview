# Solution Documentation

**Candidate Name:** Sagar Ghangale 
**Completion Date:** 06/07/2026

---

## Problems Identified

_Describe the issues you found in the original implementation. Consider aspects like:_
- Architecture and design patterns
- Code quality and maintainability
- Security vulnerabilities
- Performance concerns
- Testing gaps

Architecture and Design Patterns
The TodoService class handled both business logic and database operations, violating the Single Responsibility Principle (SRP).
The database connection string was hardcoded, making it difficult to change environments.
Dependency Injection was not used for the service, resulting in tight coupling.
No interface (ITodoService) was defined, making unit testing and mocking difficult.

Code Quality and Maintainability
Database connection code was repeated in every CRUD method.
SQL queries were built using string interpolation instead of parameters.
No exception handling was implemented for database operations.
Update operations returned success even when no record was updated.
Configuration values were hardcoded instead of being stored in configuration files.

Security Vulnerabilities
The application was vulnerable to SQL Injection because SQL statements were created using string interpolation.
No input validation was performed on the Todo model.
User input was directly included in SQL statements without sanitization.

Performance Concerns
Every request created a new database connection.
Database operations were synchronous instead of asynchronous.
GetAllTodos() retrieved all records without pagination or filtering.
No logging or monitoring was implemented for failures.

Testing Gaps
Unit tests depended on the actual SQLite database (todos.db).
Tests were not isolated and could affect one another.
Negative test cases (invalid IDs, empty database, invalid input) were missing.
Some tests assumed records already existed in the database.
Controller testing did not use Dependency Injection or mocking.

---

## Architectural Decisions

_Explain the architecture you chose and why. Consider:_
- Design patterns applied
- Project structure changes
- Technology choices
- Separation of concerns

Design Patterns Applied
Used Dependency Injection to register TodoService.
Used a Service Layer to separate API controllers from business logic.
Used Parameterized SQL Queries to prevent SQL Injection.

Project Structure Changes
Registered TodoService using the built-in Dependency Injection container.
Added automatic database initialization.
Improved CRUD methods to validate affected rows.
Replaced string interpolation with parameterized SQL queries.

Technology Choices
ASP.NET Core Web API
SQLite Database
Microsoft.Data.Sqlite
xUnit for testing
Swagger/OpenAPI for API documentation

Separation of Concerns
Controllers handle HTTP requests and responses.
TodoService contains business and database logic.
Database initialization is centralized.
Unit tests focus on validating service functionality.

---

## Trade-offs

_Discuss compromises you made and the reasoning behind them. Consider:_
- What did you prioritize?
- What did you defer or simplify?
- What alternatives did you consider?

What did you prioritize?
Secure database access using parameterized queries.
Cleaner and more maintainable code.
Reliable CRUD operations.
Better unit test coverage.
Minimal changes while improving the existing implementation.

What did you defer or simplify?
Repository Pattern
Entity Framework Core
Async database operations
Authentication and Authorization
Global Exception Handling
Logging framework integration
What alternatives did you consider?

Using Entity Framework Core instead of raw SQL.
Implementing Repository and Unit of Work patterns.
Using an in-memory SQLite database for isolated testing.

---

## How to Run

### Prerequisites
NET 8 SDK (or project target framework)
Visual Studio 2022 / Visual Studio Code
SQLite (handled automatically by Microsoft.Data.Sqlite)

### Build
```bash
dotnet restore 
dotnet build
```

### Run
```bash
#dotnet run
```

### Test
```bash
# dotnet test
```

---

## API Documentation

### Endpoints

#### Create TODO
```
Method: Post
URL: /api/createTodo
Request Body: {
  "id": 0,
  "title": "string",
  "description": "string",
  "isCompleted": true,
  "createdAt": "2026-07-06T07:13:22.339Z"
}
Response: {
  "id": 1,
  "title": "string",
  "description": "string",
  "isCompleted": true,
  "createdAt": "2026-07-06T07:13:25.5421243Z"
}
```

#### Get TODO(s)
```
Method: Get
URL: /api/getTodo?id=1
Request: id=1
Response: { "id": 1, "title": "Learn ASP.NET Core", "description": "Practice CRUD API", "isCompleted": false, "createdAt": "2026-07-06T12:00:00Z" }
```

#### Update TODO
```
Method: Put
URL: api/updateTodo
Request Body: {
  "id":1,
  "title": "string",
  "description": "string",
  "isCompleted": true
}
Response: [example]
```

#### Delete TODO
```
Method: Delete
URL: api/deleteTodo
Request: {
  "id": 1
}
Response: {
  "message": "Todo deleted successfully"
}
```

---

## Future Improvements

_What would you do if you had more time? Consider:_
- Additional features
- Performance optimizations
- Enhanced testing
- Better documentation
- Deployment considerations
- 
If more time were available, I would implement the following improvements:

Implement Repository and Unit of Work patterns.
Use Entity Framework Core with Code First Migrations.
Convert database operations to asynchronous methods (async/await).
Add FluentValidation for request validation.
Implement global exception handling middleware.
Add structured logging using Serilog.
Add JWT Authentication and Role-Based Authorization.
Improve unit testing using Moq and an in-memory SQLite database.
Configure CI/CD using GitHub Actions or Azure DevOps.
Deploy the application to Azure App Service with Application Insights monitoring.
