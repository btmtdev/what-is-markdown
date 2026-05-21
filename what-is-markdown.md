# What is Markdown?

Markdown is a lightweight markup language created by John Gruber in 2004. It uses plain text formatting syntax that can be converted to HTML and many other formats. Files use the `.md` or `.markdown` extension.

## Why Do We Need Markdown?

- **Readable as plain text** — no special software required to read or write
- **Portable** — works across all platforms and editors
- **Version control friendly** — diffs are clean and meaningful in Git
- **Widely supported** — GitHub, GitLab, Notion, Obsidian, VS Code, and thousands of tools render it natively
- **Separation of content and style** — focus on writing, not formatting

## Multi-Purpose Examples

| Purpose | Example |
|---------|---------|
| Documentation | `README.md`, API docs, user guides |
| Note-taking | Personal knowledge bases (Obsidian, Logseq) |
| Blogging | Static site generators (Hugo, Jekyll, Astro) |
| Project management | GitHub Issues, PR descriptions |
| Presentations | Marp, Slidev, reveal.js |
| Books & eBooks | mdBook, Pandoc to PDF/EPUB |
| Specifications | RFCs, design documents, ADRs |

## Why Markdown Matters for AI Agents

AI agents (like Kiro, Copilot, Cursor, etc.) rely heavily on markdown files for context and instructions:

| File | Purpose |
|------|---------|
| `README.md` | Project overview and setup instructions |
| `CONTRIBUTING.md` | Contribution guidelines |
| `CHANGELOG.md` | Version history |
| `.kiro/specs/*.md` | Feature specifications for Kiro agent |
| `.kiro/steering/*.md` | Behavioral rules and coding standards |
| `.github/copilot-instructions.md` | Custom instructions for GitHub Copilot |
| `.cursorrules` / `.cursor/rules/*.md` | Rules for Cursor AI |
| `AGENTS.md` | Instructions for OpenAI Codex agent |
| `CLAUDE.md` | Instructions for Claude Code agent |

### Why AI Agents Prefer Markdown

1. **Structured yet simple** — headings, lists, and tables give hierarchy without complex parsing
2. **Token-efficient** — no verbose XML/HTML tags wasting context window
3. **Human-editable** — developers can easily write and maintain AI instructions
4. **Lives in the repo** — version-controlled alongside code, always up to date
5. **Universal format** — every AI tool understands it without special adapters

> Markdown is the lingua franca between humans and AI agents in software development.
