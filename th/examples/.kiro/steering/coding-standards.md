# Steering: มาตรฐานการเขียนโค้ด

## ภาษาและ Framework

- TypeScript 5.x เปิด strict mode
- React 19 สำหรับ frontend components
- Express 5 สำหรับ backend API

## การตั้งชื่อ

- ไฟล์: `kebab-case.ts`
- Components: `PascalCase.tsx`
- ฟังก์ชัน/ตัวแปร: `camelCase`
- ค่าคงที่: `UPPER_SNAKE_CASE`
- Types/Interfaces: `PascalCase` (ไม่ใส่ `I` นำหน้า)

## รูปแบบโค้ด

- ใช้ composition แทน inheritance
- ใช้ early returns เพื่อลด nesting
- ความยาวฟังก์ชันสูงสุด: 30 บรรทัด
- ความยาวไฟล์สูงสุด: 200 บรรทัด

## การ Import

```typescript
// 1. External packages
import express from 'express';

// 2. Internal modules (absolute paths)
import { UserService } from '@/services/user.service';

// 3. Relative imports
import { helper } from './helper';
```

## การจัดการ Error

- ใช้ custom error classes ที่ extend `AppError`
- ห้ามกลืน error เงียบๆ
- Log error พร้อมบริบทเสมอ

## การทดสอบ

- ไฟล์เทสต์อยู่ข้างๆ source: `user.service.ts` → `user.service.test.ts`
- ใช้ `describe` / `it` blocks
- หนึ่ง assertion ต่อหนึ่งเทสต์เมื่อเป็นไปได้
- Mock external dependencies ไม่ใช่ internal modules
