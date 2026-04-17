# resume-optimize-skill

> An AI skill that converts any PDF resume into a polished, print-ready two-page A4 HTML file — with STAR-principle rewrites, beautiful typography, and Playwright-verified layout.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Works with](https://img.shields.io/badge/works%20with-Claude%20Code%20%7C%20Codex%20%7C%20OpenCode-green)

---

## What it does

1. **Extracts** all content from a PDF (or pasted plain text)
2. **Rewrites** every experience bullet using the STAR principle — Situation, Task, Action, Result
3. **Builds** a complete two-page A4 HTML resume from a built-in template (no design work needed)
4. **Verifies** layout with Playwright MCP — measures pixel overflow on both pages and auto-fixes until clean
5. **Delivers** a single `姓名简历.html` file, ready to print or export to PDF

### Output layout

Page 1: Header · Education · Skills · Internship · Project 1  
Page 2: Projects (cont.) · Awards · Self-evaluation

Typography: [Cormorant](https://fonts.google.com/specimen/Cormorant) for headings · [DM Sans](https://fonts.google.com/specimen/DM+Sans) for numbers/Latin · [Noto Serif SC](https://fonts.google.com/noto/specimen/Noto+Serif+SC) for Chinese body text

---

## Installation

### Claude Code

```bash
# Global (available in all projects)
cp resume-optimize.md ~/.claude/skills/

# Project-local
cp resume-optimize.md .claude/skills/
```

Then invoke with:
```
/resume-optimize
```

### Codex / OpenCode / any MCP-capable AI

Copy the full contents of `resume-optimize.md` (everything after the frontmatter `---`) and paste it as a system prompt or user message before sharing your resume content.

### Cursor / Windsurf / other editors

Add `resume-optimize.md` as a Rule or Context file in your editor's AI settings.

---

## Usage

### From a PDF file

```
/resume-optimize

Here is my resume: /path/to/my-resume.pdf
```

### From pasted text

```
/resume-optimize

Here is my resume:

姓名：张三
电话：138xxxxxxxx
...
```

### Adding a new experience to an existing HTML resume

```
/resume-optimize

My current resume: /path/to/resume.html

Please add this internship:
公司：某科技有限公司
项目：用户行为分析平台
时间：2025.06 — 2025.09
技术栈：Python / Spark / Kafka / Redis
...
```

---

## STAR Principle — Quick Reference

Every bullet is rewritten as:

```
【Module Tag】Situation/Task → Action (specific tech) → Result (bolded)
```

| Before | After |
|--------|-------|
| 利用 Redis 构建缓存层，减少数据库压力 | **【缓存优化】** 面对高并发查询压力，利用 Redis 构建缓存层降低数据库负载，查询响应从 **500ms 降至 50ms** |
| 实现用户认证模块 | **【权限控制】** 为实现差异化访问控制，基于 JWT + bcrypt 构建无状态认证与 **RBAC 角色权限体系** |

Forbidden weak openers: 负责、完成、实现了、参与、具有丰富经验

---

## Layout Verification

If you have [Playwright MCP](https://github.com/microsoft/playwright-mcp) configured, the skill will:

1. Open the HTML file at 1200px+ viewport
2. Measure content height vs A4 height on each page
3. Auto-adjust spacing or redistribute projects until both pages are in `[-120px, 0px]`
4. Take screenshots for visual confirmation

Without Playwright, the skill falls back to manual pixel estimation and instructs you to verify in browser.

---

## Requirements

- An AI assistant with file read/write access (Claude Code, Codex, OpenCode, etc.)
- [Playwright MCP](https://github.com/microsoft/playwright-mcp) *(optional, for layout verification)*
- Internet access for Google Fonts *(optional, fonts degrade gracefully offline)*

---

## License

MIT — see [LICENSE](LICENSE)
