# AGENTS.md

คำสั่งสำหรับ AI coding agents (OpenAI Codex ฯลฯ) ที่ทำงานใน repository นี้

## ภาพรวมโปรเจค

นี่คือ TypeScript REST API ใช้ Express และ PostgreSQL

## สไตล์โค้ด

- ใช้ TypeScript strict mode
- ใช้ `async/await` แทน callbacks
- ใช้ named exports ไม่ใช่ default exports
- ทำตาม pattern ที่มีอยู่ใน codebase

## สถาปัตยกรรม

- `src/routes/` — Express route handlers
- `src/services/` — Business logic (ไม่เกี่ยวกับ HTTP)
- `src/repositories/` — Database queries
- `src/middleware/` — Express middleware
- `src/types/` — Shared TypeScript types

## กฎ

- ห้าม commit ไฟล์ `.env`
- Endpoint ใหม่ทุกตัวต้องมี input validation ด้วย Zod
- Database query ทุกตัวต้องใช้ parameterized statements
- เขียน unit tests สำหรับ services, integration tests สำหรับ routes
- รัน `npm run lint && npm run test` ก่อนถือว่างานเสร็จ

## การทดสอบ

```bash
npm run test          # unit tests
npm run test:e2e      # integration tests
```

## ข้อควรระวัง

- ตาราง `users` ใช้ soft-delete (`deleted_at`) ต้อง filter ด้วย `WHERE deleted_at IS NULL` เสมอ
- Auth tokens หมดอายุใน 15 นาที Refresh tokens อยู่ได้ 7 วัน
