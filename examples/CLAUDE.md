# CLAUDE.md

Instructions for Claude Code agent working in this repository.

## Project

E-commerce API built with Node.js, Express, and MongoDB.

## Commands

- Build: `npm run build`
- Test: `npm run test`
- Lint: `npm run lint`
- Single test: `npm run test -- --grep "test name"`

## Style Guide

- Use functional programming patterns where possible
- Error handling: throw custom errors from `src/errors/`, caught by global middleware
- Logging: use the `logger` instance from `src/utils/logger.ts`
- No `any` types — use `unknown` and narrow

## Important Context

- We use MongoDB transactions for order processing — always use `session` parameter
- The `price` field is stored in cents (integer) to avoid floating point issues
- All dates are stored as UTC ISO strings

## Do Not

- Do not modify `src/config/production.ts` without asking
- Do not add new dependencies without justification
- Do not use `console.log` — use the logger
