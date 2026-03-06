# TodoList Demo App

## Product Overview

A simple TodoList application for demonstrating the Spec-Driven Development Workflow. Users can create, view, filter, complete, and delete todos.

## Target Users

Single-role: general users who need a basic task management tool.

## Tech Stack

- **Frontend**: React + Vite + TypeScript
- **Backend**: Hono + TypeScript, in-memory store
- **Shared**: REST API over JSON, UUID v4 for IDs

## Naming Conventions

- Feature IDs: kebab-case (e.g., `todo`, `user-login`)
- API paths: `/api/<resource>` (plural, lowercase)
- TypeScript: PascalCase for types/interfaces, camelCase for variables/functions
- Components: PascalCase file names (e.g., `TodoItem.tsx`)

## Domain Glossary

| Term | Definition |
|------|-----------|
| Todo | A task item with title, optional description, and completion status |
| Active | A todo that has not been completed |
| Completed | A todo that has been marked as done |
| Filter | A view mode to show all, active, or completed todos |
