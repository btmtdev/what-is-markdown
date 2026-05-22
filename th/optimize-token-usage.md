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

---

## แนวทางตามบทบาท (Role-Based Tips)

### 1. Code Developer

| ✅ ควรทำ | ❌ ไม่ควรทำ |
|---------|-----------|
| บอก tech stack สั้นๆ: `TypeScript, React, PostgreSQL` | อธิบายว่า React คืออะไร |
| ให้ command ที่ใช้: `npm test`, `npm run lint` | เขียนขั้นตอน setup ยาว 20 บรรทัด |
| ระบุ pattern: "ใช้ repository pattern" | อธิบาย design pattern ทั้งหมด |
| บอกข้อยกเว้น: "price เก็บเป็น cents" | บอกสิ่งที่เป็น common sense |
| แปะ path สำคัญ: `src/errors/`, `src/config/` | ลิสต์ทุกไฟล์ใน project |

**ตัวอย่างที่ดี:**
```markdown
## Stack
TypeScript 5, Next.js 15, Prisma, PostgreSQL

## Commands
- Test: `pnpm test`
- Lint: `pnpm lint`
- DB migrate: `pnpm prisma migrate dev`

## Rules
- No `any`, use Zod for validation
- Errors: throw from `src/lib/errors.ts`
- API responses: `{ data, error, meta }` format
```

---

### 2. Network Engineer

| ✅ ควรทำ | ❌ ไม่ควรทำ |
|---------|-----------|
| ระบุ vendor/platform: `Cisco IOS-XE`, `Palo Alto` | อธิบายว่า VLAN คืออะไร |
| บอก naming convention: `SW-BKK-FL3-01` | ใส่ topology ทั้งหมดในไฟล์ |
| ให้ template command สั้นๆ | Copy config ยาว 200 บรรทัดมาใส่ |
| ระบุ subnet scheme: `10.{site}.{vlan}.0/24` | อธิบาย subnetting ตั้งแต่ต้น |
| บอกข้อห้าม: "ห้ามแก้ core switch โดยไม่มี change request" | ใส่ policy document ทั้งเล่ม |

**ตัวอย่างที่ดี:**
```markdown
## Environment
- Core: Cisco Nexus 9000 (NX-OS)
- Firewall: Palo Alto PA-5200
- SD-WAN: Cisco Viptela

## Naming
- Switch: `SW-{SITE}-FL{FLOOR}-{SEQ}` (e.g., SW-BKK-FL3-01)
- VLAN: 10=Mgmt, 20=Server, 30=User, 99=Native

## Do Not
- Change ACL on production FW without CR number
- Use VLAN 1 for any traffic
- Assign /32 to server interfaces (use /27 minimum)
```

---

### 3. Helpdesk / IT Support

| ✅ ควรทำ | ❌ ไม่ควรทำ |
|---------|-----------|
| บอก tool ที่ใช้: `ServiceNow`, `Jira Service Desk` | อธิบายวิธีใช้ tool ทั้งหมด |
| ให้ template ตอบ user สั้นๆ | ใส่ script ตอบทุก scenario |
| ระบุ SLA: P1=1hr, P2=4hr | Copy SLA document ทั้งหมด |
| บอก escalation path สั้นๆ | ใส่ org chart |
| ให้ troubleshooting checklist | เขียน KB article ยาวๆ |

**ตัวอย่างที่ดี:**
```markdown
## Tools
- Ticketing: ServiceNow (SNOW)
- Remote: Bomgar / Quick Assist
- Asset: CMDB in SNOW

## SLA
- P1 (Critical): respond 15min, resolve 1hr
- P2 (High): respond 30min, resolve 4hr
- P3 (Medium): respond 2hr, resolve 1 business day

## Escalation
- L1 → L2: after 30min no resolution
- L2 → L3: requires code change or infra access
- L3 → Vendor: hardware failure or license issue

## Response Template
"สวัสดีครับ/ค่ะ ได้รับ ticket #{number} แล้ว กำลังดำเนินการ
จะอัปเดตภายใน {SLA time} ครับ/ค่ะ"
```

---

### 4. Infrastructure Team

| ✅ ควรทำ | ❌ ไม่ควรทำ |
|---------|-----------|
| ระบุ cloud/on-prem: `AWS ap-southeast-1`, `VMware 8` | อธิบายว่า cloud คืออะไร |
| บอก IaC tool: `Terraform`, `Ansible` | ใส่ playbook ทั้งหมด |
| ให้ naming convention: `prod-web-sg-01` | ใส่ inventory ทุก server |
| ระบุ tagging policy สั้นๆ | Copy governance document |
| บอก critical path: "ห้าม destroy RDS production" | ลิสต์ทุก resource |

**ตัวอย่างที่ดี:**
```markdown
## Platform
- Cloud: AWS (ap-southeast-1)
- IaC: Terraform 1.8 + Atlantis
- Config: Ansible 2.16
- Container: EKS 1.30

## Naming
- Resource: `{env}-{service}-{type}-{seq}`
- Example: `prod-api-ec2-01`, `stg-web-alb-01`

## Tags (required)
- Environment, Service, Owner, CostCenter

## Do Not
- `terraform destroy` on prod without approval
- Create resources outside Terraform
- Use default VPC or default security group
- Open 0.0.0.0/0 on any port except 443
```
