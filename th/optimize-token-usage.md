# วิธีเขียน CLAUDE.md ให้ประหยัด Token

สำหรับผู้ใช้ Claude Enterprise/Team — เทคนิคเขียน markdown instructions ให้กระชับ ลดการใช้ token ต่อ session

---

## ทำไม Token ถึงสำคัญ?

- CLAUDE.md ถูกโหลดเข้า context **ทุกครั้ง** ที่เริ่ม session
- ยิ่งไฟล์ยาว → ยิ่งกิน token → ยิ่งเหลือพื้นที่น้อยสำหรับงานจริง
- Claude Enterprise คิดค่าใช้จ่ายตาม token ที่ใช้

## กฎ 4 ข้อ (จาก Andrej Karpathy)

1. **เขียนโค้ดให้น้อยที่สุด** — บอก agent ว่า "เขียนแค่ที่จำเป็น"
2. **อ่านก่อนเขียน** — บอกให้อ่าน codebase ก่อนแก้ไข
3. **ทดสอบเสมอ** — บอกให้รัน test หลังทุกการเปลี่ยนแปลง
4. **Commit บ่อยๆ** — บอกให้ commit ทีละ feature เล็กๆ

## เทคนิคประหยัด Token

### ❌ แบบยาว (กิน token)

```markdown
## Code Style Guidelines

When writing code for this project, please ensure that you follow
these guidelines carefully. We use TypeScript with strict mode enabled.
All functions should have proper type annotations. Please do not use
the `any` type under any circumstances. Instead, use `unknown` and
narrow the type appropriately. We prefer functional programming
patterns where possible, and we use early returns to reduce nesting.
Our maximum function length is 30 lines.
```

### ✅ แบบกระชับ (ประหยัด token)

```markdown
## Style
- TypeScript strict, no `any`
- Functional patterns, early returns
- Max 30 lines/function
```

### เปรียบเทียบ

| แบบ | Token โดยประมาณ | ลดได้ |
|-----|----------------|-------|
| ยาว (ย่อหน้า) | ~80 tokens | — |
| กระชับ (bullet) | ~25 tokens | 70% |

## โครงสร้าง CLAUDE.md ที่ประหยัด

```markdown
# CLAUDE.md

## Commands
- Test: `npm test`
- Lint: `npm run lint`
- Build: `npm run build`

## Rules
- TypeScript strict, no `any`
- Errors: custom classes in `src/errors/`
- Prices: stored in cents (integer)
- Dates: UTC ISO strings

## Do Not
- Modify `src/config/production.ts`
- Add deps without justification
- Use `console.log` (use logger)
```

**~60 tokens** — ได้ข้อมูลครบ

## สิ่งที่ควรทำ

| ✅ ทำ | ❌ ไม่ทำ |
|-------|---------|
| ใช้ bullet points | เขียนเป็นย่อหน้ายาว |
| เขียนแค่กฎที่ agent ต้องรู้ | อธิบาย "ทำไม" ยาวๆ |
| ใช้ backtick สำหรับ command | เขียน command ซ้ำหลายที่ |
| เก็บไว้ < 100 บรรทัด | ใส่ทุกอย่างที่นึกออก |
| แยกไฟล์ถ้ายาวเกิน | ยัดทุกอย่างในไฟล์เดียว |

## เทคนิคเพิ่มเติม

### 1. ใช้ .kiro/steering/ แยกไฟล์ (สำหรับ Kiro)

แทนที่จะยัดทุกอย่างในไฟล์เดียว แยกเป็น:
```
.kiro/steering/
├── style.md          # 20 บรรทัด
├── testing.md        # 15 บรรทัด
└── architecture.md   # 25 บรรทัด
```
Kiro จะโหลดเฉพาะไฟล์ที่เกี่ยวข้อง

### 2. ลบสิ่งที่ agent รู้อยู่แล้ว

ไม่ต้องบอก:
- "ใช้ meaningful variable names" (agent ทำอยู่แล้ว)
- "เขียน clean code" (กว้างเกินไป)
- "ทำตาม best practices" (ไม่มีข้อมูลใหม่)

### 3. ใช้ตัวอย่างแทนคำอธิบาย

```markdown
# ❌ อธิบายยาว
Error handling ต้องใช้ custom error class ที่ extend จาก AppError
โดยต้อง throw error ที่มี status code และ message ที่ชัดเจน

# ✅ ให้ตัวอย่างสั้นๆ
Errors: `throw new AppError(404, "User not found")`
```

## สรุป

> เขียน CLAUDE.md เหมือนเขียน Tweet — สั้น ชัด ได้ใจความ
>
> ทุก token ที่ประหยัดได้ = พื้นที่เพิ่มสำหรับงานจริง
