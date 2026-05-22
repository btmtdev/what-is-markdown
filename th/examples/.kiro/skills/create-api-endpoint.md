# Skill: สร้าง API Endpoint

## คำอธิบาย
สร้าง REST API endpoint ใหม่ตาม convention ของโปรเจค

## Input
- `resource`: ชื่อ resource (เช่น "products")
- `methods`: HTTP methods ที่ต้องรองรับ (เช่น GET, POST, PUT, DELETE)

## ขั้นตอน

1. สร้างไฟล์ route ที่ `src/routes/{resource}.ts`
2. สร้างไฟล์ service ที่ `src/services/{resource}.service.ts`
3. สร้างไฟล์ repository ที่ `src/repositories/{resource}.repository.ts`
4. สร้าง Zod validation schema ที่ `src/schemas/{resource}.schema.ts`
5. สร้างไฟล์เทสต์ที่ `tests/{resource}.test.ts`
6. ลงทะเบียน route ใน `src/routes/index.ts`

## Template: ไฟล์ Route

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

## การตรวจสอบ

- รัน `npm run lint` หลังสร้างไฟล์
- รัน `npm run test` เพื่อยืนยันว่าไม่มี regression
