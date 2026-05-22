# Markdown คืออะไร?

Markdown คือภาษา markup น้ำหนักเบา สร้างโดย John Gruber ในปี 2004 ใช้รูปแบบ plain text ที่สามารถแปลงเป็น HTML และรูปแบบอื่นๆ ได้ ไฟล์ใช้นามสกุล `.md` หรือ `.markdown`

## ทำไมต้องใช้ Markdown?

- **อ่านได้ทันทีแม้เป็น plain text** — ไม่ต้องใช้โปรแกรมพิเศษ
- **ใช้ได้ทุกแพลตฟอร์ม** — Windows, Mac, Linux, มือถือ
- **เหมาะกับ Version Control** — diff ใน Git อ่านง่าย
- **รองรับทุกที่** — GitHub, GitLab, Notion, Obsidian, VS Code
- **แยกเนื้อหาออกจากรูปแบบ** — โฟกัสที่การเขียน ไม่ใช่การจัดหน้า

## ตัวอย่างการใช้งานหลากหลาย

| การใช้งาน | ตัวอย่าง |
|-----------|----------|
| เอกสารโปรเจค | `README.md`, API docs, คู่มือผู้ใช้ |
| จดบันทึก | Obsidian, Logseq, Notion |
| เขียนบล็อก | Hugo, Jekyll, Astro |
| จัดการโปรเจค | GitHub Issues, PR descriptions |
| นำเสนอ | Marp, Slidev, reveal.js |
| เขียนหนังสือ | mdBook, Pandoc แปลงเป็น PDF/EPUB |
| ข้อกำหนดระบบ | RFCs, design documents, ADRs |

## ทำไม Markdown ถึงสำคัญสำหรับ AI Agent?

AI Agent (เช่น Kiro, Copilot, Cursor) ใช้ไฟล์ markdown เป็นบริบทและคำสั่ง:

| ไฟล์ | วัตถุประสงค์ |
|------|-------------|
| `README.md` | ภาพรวมโปรเจคและวิธีติดตั้ง |
| `CONTRIBUTING.md` | แนวทางการ contribute |
| `CHANGELOG.md` | ประวัติเวอร์ชัน |
| `.kiro/specs/*.md` | สเปคฟีเจอร์สำหรับ Kiro |
| `.kiro/steering/*.md` | กฎและมาตรฐานโค้ดสำหรับ Kiro |
| `.github/copilot-instructions.md` | คำสั่งสำหรับ GitHub Copilot |
| `.cursorrules` / `.cursor/rules/*.md` | กฎสำหรับ Cursor AI |
| `AGENTS.md` | คำสั่งสำหรับ OpenAI Codex |
| `CLAUDE.md` | คำสั่งสำหรับ Claude Code |

### ทำไม AI Agent ถึงชอบ Markdown?

1. **มีโครงสร้างแต่เรียบง่าย** — หัวข้อ, รายการ, ตาราง ให้ลำดับชั้นโดยไม่ต้อง parse ซับซ้อน
2. **ประหยัด Token** — ไม่มี XML/HTML tag ยาวๆ กิน context window
3. **มนุษย์แก้ไขง่าย** — นักพัฒนาเขียนและดูแลคำสั่ง AI ได้สะดวก
4. **อยู่ใน repo** — version control ร่วมกับโค้ด อัปเดตตลอด
5. **รูปแบบสากล** — AI ทุกตัวเข้าใจโดยไม่ต้องมี adapter พิเศษ

> Markdown คือภาษากลางระหว่างมนุษย์กับ AI Agent ในการพัฒนาซอฟต์แวร์
