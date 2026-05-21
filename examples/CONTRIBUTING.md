# Contributing

Thank you for contributing! Here's how to get started.

## Development Setup

1. Fork and clone the repository
2. Install dependencies: `npm install`
3. Copy environment file: `cp .env.example .env`
4. Start development: `npm run dev`

## Branch Naming

```
feature/short-description
fix/issue-number-description
docs/what-changed
```

## Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add user search endpoint
fix: resolve memory leak in cache layer
docs: update API documentation
test: add integration tests for auth
refactor: simplify error handling middleware
```

## Pull Request Process

1. Create a feature branch from `main`
2. Make your changes with tests
3. Ensure all checks pass: `npm run lint && npm run test`
4. Open a PR with a clear description
5. Request review from at least one maintainer

## Code Review Checklist

- [ ] Tests pass locally
- [ ] No linting errors
- [ ] New code has test coverage
- [ ] Documentation updated if needed
- [ ] No secrets or credentials committed

## Reporting Issues

Use GitHub Issues with:
- Clear title
- Steps to reproduce
- Expected vs actual behavior
- Environment details (OS, Node version)
