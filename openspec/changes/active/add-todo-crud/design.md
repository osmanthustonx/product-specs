# Design: TodoList CRUD

## API Design
| Method | Path | Response |
|--------|------|----------|
| POST | /api/todos | 201 |
| GET | /api/todos?filter= | 200 |
| PATCH | /api/todos/:id | 200 (toggle) |
| DELETE | /api/todos/:id | 200 |

## Data Model
Todo { id, title, description?, completed, createdAt, updatedAt }

## State Machine
[active] ←→ [completed] (via PATCH toggle)
