# โปรเจคของฉัน

คำอธิบายสั้นๆ ว่าโปรเจคนี้ทำอะไร

## เริ่มต้นใช้งาน

### สิ่งที่ต้องมี

- Node.js >= 18
- npm หรือ pnpm

### การติดตั้ง

```bash
git clone https://github.com/username/my-project.git
cd my-project
npm install
```

### การใช้งาน

```bash
npm run dev
```

เปิด [http://localhost:3000](http://localhost:3000) ในเบราว์เซอร์

## โครงสร้างโปรเจค

```
src/
├── components/   # UI components
├── services/     # Business logic
├── utils/        # ฟังก์ชันช่วยเหลือ
└── index.ts      # จุดเริ่มต้น
```

## คำสั่ง

| คำสั่ง | รายละเอียด |
|--------|-----------|
| `npm run dev` | เริ่ม development server |
| `npm run build` | Build สำหรับ production |
| `npm run test` | รันเทสต์ |
| `npm run lint` | รัน linter |

## การมีส่วนร่วม

ดู [CONTRIBUTING.md](CONTRIBUTING.md) สำหรับแนวทาง

## สัญญาอนุญาต

MIT
