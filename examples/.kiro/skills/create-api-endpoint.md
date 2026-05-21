# Skill: Create API Endpoint

## Description
Creates a new REST API endpoint following project conventions.

## Input
- `resource`: Name of the resource (e.g., "products")
- `methods`: HTTP methods to support (e.g., GET, POST, PUT, DELETE)

## Steps

1. Create route file at `src/routes/{resource}.ts`
2. Create service file at `src/services/{resource}.service.ts`
3. Create repository file at `src/repositories/{resource}.repository.ts`
4. Create Zod validation schema at `src/schemas/{resource}.schema.ts`
5. Create test file at `tests/{resource}.test.ts`
6. Register route in `src/routes/index.ts`

## Template: Route File

```typescript
import { Router } from 'express';
import { validate } from '../middleware/validate';
import { {Resource}Schema } from '../schemas/{resource}.schema';
import * as service from '../services/{resource}.service';

const router = Router();

router.get('/', async (req, res, next) => {
  try {
    const result = await service.findAll();
    res.json(result);
  } catch (err) {
    next(err);
  }
});

router.post('/', validate({Resource}Schema), async (req, res, next) => {
  try {
    const result = await service.create(req.body);
    res.status(201).json(result);
  } catch (err) {
    next(err);
  }
});

export default router;
```

## Validation

- Run `npm run lint` after generation
- Run `npm run test` to verify no regressions
