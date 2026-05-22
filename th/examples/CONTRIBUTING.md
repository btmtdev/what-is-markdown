# การมีส่วนร่วม

ขอบคุณที่สนใจมีส่วนร่วม! นี่คือวิธีเริ่มต้น

## การตั้งค่าสำหรับพัฒนา

1. Fork และ clone repository
2. ติดตั้ง dependencies: `npm install`
3. คัดลอกไฟล์ environment: `cp .env.example .env`
4. เริ่มพัฒนา: `npm run dev`

## การตั้งชื่อ Branch

```
feature/คำอธิบายสั้นๆ
fix/หมายเลข-issue-คำอธิบาย
docs/สิ่งที่เปลี่ยน
```

## ข้อความ Commit

ใช้ [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: เพิ่ม endpoint ค้นหาผู้ใช้
fix: แก้ memory leak ใน cache layer
docs: อัปเดตเอกสาร API
test: เพิ่ม integration tests สำหรับ auth
refactor: ทำให้ error handling middleware เรียบง่ายขึ้น
```

## ขั้นตอน Pull Request

1. สร้าง feature branch จาก `main`
2. ทำการเปลี่ยนแปลงพร้อมเทสต์
3. ตรวจสอบว่าผ่านทุกอย่าง: `npm run lint && npm run test`
4. เปิด PR พร้อมคำอธิบายที่ชัดเจน
5. ขอ review จาก maintainer อย่างน้อย 1 คน

## Checklist สำหรับ Code Review

- [ ] เทสต์ผ่านในเครื่อง
- [ ] ไม่มี linting errors
- [ ] โค้ดใหม่มี test coverage
- [ ] อัปเดตเอกสารถ้าจำเป็น
- [ ] ไม่มี secrets หรือ credentials ถูก commit

## การรายงานปัญหา

ใช้ GitHub Issues พร้อม:
- หัวข้อที่ชัดเจน
- ขั้นตอนในการทำซ้ำ
- พฤติกรรมที่คาดหวัง vs ที่เกิดขึ้นจริง
- รายละเอียดสภาพแวดล้อม (OS, Node version)
