# CLAUDE.md

คำสั่งสำหรับ Claude Code agent ที่ทำงานใน repository นี้

## โปรเจค

E-commerce API สร้างด้วย Node.js, Express และ MongoDB

## คำสั่ง

- Build: `npm run build`
- Test: `npm run test`
- Lint: `npm run lint`
- เทสต์เดี่ยว: `npm run test -- --grep "ชื่อเทสต์"`

## แนวทางการเขียนโค้ด

- ใช้ functional programming patterns เมื่อเป็นไปได้
- Error handling: throw custom errors จาก `src/errors/` จับด้วย global middleware
- Logging: ใช้ `logger` instance จาก `src/utils/logger.ts`
- ห้ามใช้ `any` type — ใช้ `unknown` แล้ว narrow

## บริบทสำคัญ

- ใช้ MongoDB transactions สำหรับการประมวลผลคำสั่งซื้อ — ต้องใช้ `session` parameter เสมอ
- ฟิลด์ `price` เก็บเป็นสตางค์ (integer) เพื่อหลีกเลี่ยงปัญหา floating point
- วันที่ทั้งหมดเก็บเป็น UTC ISO strings

## ห้ามทำ

- ห้ามแก้ไข `src/config/production.ts` โดยไม่ถาม
- ห้ามเพิ่ม dependencies ใหม่โดยไม่มีเหตุผล
- ห้ามใช้ `console.log` — ใช้ logger
