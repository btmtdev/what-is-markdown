# Steering: Coding Standards

## Language & Framework

- TypeScript 5.x with strict mode enabled
- React 19 for frontend components
- Express 5 for backend API

## Naming Conventions

- Files: `kebab-case.ts`
- Components: `PascalCase.tsx`
- Functions/variables: `camelCase`
- Constants: `UPPER_SNAKE_CASE`
- Types/Interfaces: `PascalCase` (no `I` prefix)

## Code Patterns

- Prefer composition over inheritance
- Use early returns to reduce nesting
- Maximum function length: 30 lines
- Maximum file length: 200 lines

## Imports

```typescript
// 1. External packages
import express from 'express';

// 2. Internal modules (absolute paths)
import { UserService } from '@/services/user.service';

// 3. Relative imports
import { helper } from './helper';
```

## Error Handling

- Use custom error classes extending `AppError`
- Never swallow errors silently
- Always log errors with context

## Testing

- Test file next to source: `user.service.ts` → `user.service.test.ts`
- Use `describe` / `it` blocks
- One assertion per test when possible
- Mock external dependencies, not internal modules
