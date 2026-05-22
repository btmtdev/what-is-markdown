# Real-World Examples from the Internet

ตัวอย่างไฟล์ AI Agent จาก repository จริงบน GitHub

---

## CLAUDE.md — ตัวอย่างจาก GitHub

| # | Repository | รายละเอียด |
|---|-----------|-----------|
| 1 | [anthropics/claude-code-action](https://github.com/anthropics/claude-code-action/blob/main/CLAUDE.md) | Official — GitHub Action สำหรับ Claude ตอบ @claude mentions |
| 2 | [josix/awesome-claude-md](https://github.com/josix/awesome-claude-md) | รวม CLAUDE.md ที่ดีที่สุดจากหลาย repo พร้อม best practices |
| 3 | [abhishekray07/claude-md-templates](https://github.com/abhishekray07/claude-md-templates) | Template collection สำหรับ CLAUDE.md หลายรูปแบบ |
| 4 | [ctoth/global CLAUDE.md](https://gist.github.com/ctoth/d8e629209ff1d9748185b9830fa4e79f) | ตัวอย่าง global CLAUDE.md สำหรับใช้ข้าม project |
| 5 | [centminmod/my-claude-code-setup](https://github.com/centminmod/my-claude-code-setup) | Starter template พร้อม memory bank system |

### แหล่งเรียนรู้เพิ่มเติม
- [Using CLAUDE.md Files — Anthropic Blog](https://www.claude.com/blog/using-claude-md-files)
- [Karpathy's CLAUDE.md 4 Rules](https://antigravity.codes/blog/karpathy-claude-code-skills-guide)

---

## AGENTS.md — ตัวอย่างจาก GitHub

| # | Repository | รายละเอียด |
|---|-----------|-----------|
| 1 | [openai/codex](https://github.com/openai/codex/blob/main/AGENTS.md) | Official — AGENTS.md ของ Codex CLI เอง (Rust project) |
| 2 | [AGENTS.md SPEC](https://gist.github.com/dpaluy/cc42d59243b0999c1b3f9cf60dfd3be6) | Spec อธิบายรูปแบบและวิธีเขียน AGENTS.md |
| 3 | [vibecoding.app/agents-md-guide](https://vibecoding.app/blog/agents-md-guide) | คู่มือ AGENTS.md สำหรับ Copilot, Cursor, Codex, Devin |
| 4 | [opentools.ai/agents-md](https://opentools.ai/resources/codex-agents-md) | Mastering AGENTS.md: Custom Instructions |
| 5 | [developertoolkit.ai/agents-md](https://developertoolkit.ai/en/codex/quick-start/agents-md/) | Quick start guide พร้อมตัวอย่าง |

### แหล่งเรียนรู้เพิ่มเติม
- [Custom instructions with AGENTS.md — OpenAI Docs](https://developers.openai.com/codex/guides/agents-md)
- [agentsmd.io](https://agentsmd.io/how-to-use-agents-md-in-codex)

---

## Kiro Skills (SKILL.md) — ตัวอย่างจาก GitHub

| # | Repository | รายละเอียด |
|---|-----------|-----------|
| 1 | [jasonkneen/kiro/skills](https://github.com/jasonkneen/kiro/blob/main/skills/README.md) | รวม skills: spec-driven-dev, requirements, design, QA, troubleshooting |
| 2 | [jasonkneen/kiro/spec-driven-development](https://github.com/jasonkneen/kiro/tree/main/skills/spec-driven-development) | Skill สำหรับ spec-driven development methodology |
| 3 | [jasonkneen/kiro/requirements-engineering](https://github.com/jasonkneen/kiro/tree/main/skills/requirements-engineering) | Skill สำหรับเขียน requirements แบบ EARS format |
| 4 | [jasonkneen/kiro/create-steering-documents](https://github.com/jasonkneen/kiro/tree/main/skills/create-steering-documents) | Skill สำหรับสร้าง .kiro/steering/ files |
| 5 | [MartinRistov/skill-to-kiro](https://github.com/MartinRistov/skill-to-kiro) | แปลง Claude SKILL.md เป็น Kiro steering files |

---

## Kiro Steering (Context) — ตัวอย่างจาก GitHub

| # | Repository | รายละเอียด |
|---|-----------|-----------|
| 1 | [aws-samples/sample-kiro-steering-studio](https://github.com/aws-samples/sample-kiro-steering-studio) | Official AWS sample — Kiro Steering Studio app |
| 2 | [awsdataarchitect/kiro-best-practices](https://github.com/awsdataarchitect/kiro-best-practices/blob/main/.kiro/steering/python-best-practices.md) | Python best practices steering file |
| 3 | [requix/azure-kiro-powers](https://github.com/requix/azure-kiro-powers) | Azure infrastructure steering สำหรับ Kiro |

### หมายเหตุ
Kiro steering files อยู่ที่ `.kiro/steering/*.md` ใน repo ทำหน้าที่เป็น "กฎ" ที่ Kiro agent จะอ่านทุกครั้งที่ทำงาน เทียบเท่ากับ CLAUDE.md หรือ AGENTS.md แต่แยกเป็นไฟล์ย่อยตามหัวข้อ

---

## สรุปเปรียบเทียบ

| ไฟล์ | AI Agent | ตำแหน่ง | ลักษณะ |
|------|----------|---------|--------|
| `CLAUDE.md` | Claude Code | root ของ repo | ไฟล์เดียว รวมทุกคำสั่ง |
| `AGENTS.md` | OpenAI Codex (+ Copilot, Cursor, Devin) | root หรือ subdirectory | ไฟล์เดียว, สามารถมีหลายไฟล์ใน subdirectory |
| `.kiro/steering/*.md` | Kiro | `.kiro/steering/` | แยกเป็นหลายไฟล์ตามหัวข้อ |
| `.kiro/skills/*.md` | Kiro | `.kiro/skills/` | Template สำหรับงานที่ทำซ้ำ |
| `.github/copilot-instructions.md` | GitHub Copilot | `.github/` | ไฟล์เดียว |
| `.cursor/rules/*.md` | Cursor | `.cursor/rules/` | แยกเป็นหลายไฟล์ |
