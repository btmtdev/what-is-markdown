# AGENTS.md

Instructions for AI coding agents (OpenAI Codex, etc.) working in this repository.

## Project Overview

This is a TypeScript REST API using Express and PostgreSQL.

## Code Style

- Use TypeScript strict mode
- Prefer `async/await` over callbacks
- Use named exports, not default exports
- Follow existing patterns in the codebase

## Architecture

- `src/routes/` — Express route handlers
- `src/services/` — Business logic (no HTTP concerns)
- `src/repositories/` — Database queries
- `src/middleware/` — Express middleware
- `src/types/` — Shared TypeScript types

## Rules

- Never commit `.env` files
- All new endpoints must have input validation using Zod
- All database queries must use parameterized statements
- Write unit tests for services, integration tests for routes
- Run `npm run lint && npm run test` before considering work complete

## Testing

```bash
npm run test          # unit tests
npm run test:e2e      # integration tests
```

## Common Pitfalls

- The `users` table has a soft-delete (`deleted_at`). Always filter by `WHERE deleted_at IS NULL`.
- Auth tokens expire in 15 minutes. Refresh tokens last 7 days.
