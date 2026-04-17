---
name: resume-optimize
description: Convert a PDF resume into a polished two-page A4 HTML resume. Applies STAR principle to all experience bullets, builds the HTML from a provided template, and uses Playwright MCP to verify layout with no overflow.
---

# Resume Optimization Skill

The user provides a PDF resume (or raw text content). Your job is to produce a complete, styled two-page A4 HTML file with all experience descriptions rewritten using the STAR principle.

**Read every instruction below before writing a single line of HTML. Do not skip phases.**

---

## Phase 0 — Extract Content from PDF

If the input is a PDF file path, extract all text content first using available tools (file read, MCP, or CLI). Organize the extracted text into these categories before proceeding:

- Personal info: name, phone, email, GitHub/blog, job target
- Education: school, major, degree, GPA, dates
- Skills: list by category
- Internship/work: company, role, dates, project name, tech stack, bullet points
- Projects: project name, dates, tech stack, description, bullet points
- Awards/certificates

If extraction produces garbled text (common with PDFs), ask the user to paste the content as plain text instead.

**Do not guess or invent any content.** If a section is missing, leave it out.

---

## Phase 1 — Apply STAR Principle to All Experience

Before writing any HTML, rewrite every bullet point from internship and project sections.

### STAR formula

```
【Module】Situation/Task → Action (specific tech/method) → Result (bolded)
```

### Rules — follow exactly

1. **Start with a bold tag in 【Chinese brackets】** that names the module (e.g. 【缓存优化】【权限控制】【数据采集】). This lets a recruiter scan in 2 seconds.
2. **S/T clause**: explain *why* this was needed — the business problem, performance bottleneck, or technical constraint. Do NOT just say "I implemented X".
3. **A clause**: name the exact technology, algorithm, or design decision. Be specific (e.g. "Redis ZSet 延迟队列" not just "队列").
4. **R clause**: end with a bolded outcome. Prefer numbers (`**500ms → 50ms**`, `**20,000+ 注册用户**`). If no numbers are available, use a qualitative result (`**杜绝超卖**`, `**覆盖 OWASP Top 10**`).
5. One bullet = one sentence. No conjunctions chaining 3+ actions together.
6. Never use: 负责、完成、实现了、参与、具有丰富经验 as the opening word. These are weak openers.

### Transformation examples

| Before | After |
|--------|-------|
| 利用 Redis 构建缓存层，减少数据库压力 | **【缓存优化】** 面对高并发查询压力，利用 Redis 构建缓存层降低数据库负载，查询响应从 **500ms 降至 50ms** |
| 实现用户认证模块 | **【权限控制】** 为实现用户与管理员差异化访问，基于 JWT + bcrypt 构建无状态认证与 **RBAC 角色权限体系**，保障接口安全 |
| 使用 goquery 解析教务系统页面 | **【数据采集】** 面对教务系统无开放 API 的现状，基于 **goquery** 解析 HTML 页面提取成绩数据，支持文字成绩自动数值转换 |

### Result-mark line

Each experience item ends with a one-line summary of skills gained. Format:
```
▸ 掌握/深入理解 X、Y、Z（不重复 bullet 内容，说收获）
```

---

## Phase 2 — Build the HTML File

Use the template below. Fill in the extracted + STAR-rewritten content. **Do not change the CSS. Do not add new CSS classes. Do not inline styles except where the template already does.**

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{{姓名}} — {{职位方向}}</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Cormorant:ital,wght@0,400;0,500;0,600;0,700;1,400;1,500&family=Cormorant+SC:wght@500;600;700&family=DM+Sans:ital,wght@0,400;0,500;0,600;0,700;1,400;1,500;1,600;1,700&family=Noto+Serif+SC:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  @page { size: A4; margin: 0; }
  * { margin: 0; padding: 0; box-sizing: border-box; }
  :root {
    --paper: #FAF8F3; --ink: #1C1714; --mid: #4A4340;
    --muted: #8A7F7A; --rule: #DDD8D0; --accent: #9B3A2A; --accent2: #C2724F;
  }
  body {
    font-family: 'DM Sans', 'Noto Serif SC', 'Songti SC', Georgia, serif;
    color: var(--ink); background: #D8D4CC;
    font-size: 12px; line-height: 1.72;
    -webkit-font-smoothing: antialiased;
  }
  a { color: var(--mid); text-decoration: none; border-bottom: 0.5px solid var(--rule); }
  .page {
    width: 210mm; height: 297mm; margin: 22px auto;
    background: var(--paper); padding: 22px 32px 20px;
    position: relative; overflow: hidden;
    box-shadow: 0 4px 24px rgba(0,0,0,0.15);
  }
  .page::before {
    content: ''; position: absolute;
    top: 10px; left: 10px; right: 10px; bottom: 10px;
    border: 0.5px solid var(--rule); pointer-events: none;
  }
  /* HEADER */
  .header {
    display: flex; gap: 16px; align-items: flex-start;
    padding-bottom: 12px; border-bottom: 1px solid var(--rule);
  }
  .avatar {
    width: 72px; height: 96px; object-fit: cover;
    object-position: center top; border-radius: 2px;
    flex-shrink: 0; filter: sepia(8%) contrast(1.02);
  }
  .header-text { flex: 1; }
  .name {
    font-family: 'DM Sans', 'Noto Serif SC', Georgia, serif;
    font-size: 24px; font-weight: 700; letter-spacing: 8px;
    color: var(--ink); line-height: 1.1; margin-bottom: 4px;
  }
  .tagline {
    font-family: 'DM Sans', 'Cormorant', Georgia, serif;
    font-size: 12px; font-weight: 500; font-style: italic;
    color: var(--accent); letter-spacing: 0.3px; margin-bottom: 6px;
  }
  .meta-row { display: flex; flex-direction: column; }
  .meta-line {
    font-size: 11px; color: var(--mid); line-height: 1.7;
    font-family: 'DM Sans', 'Noto Serif SC', serif;
  }
  .meta-line strong { color: var(--ink); }
  /* PAGE 2 MINI HEADER */
  .page2-bar {
    display: flex; justify-content: space-between; align-items: baseline;
    border-bottom: 1px solid var(--rule); padding-bottom: 6px; margin-bottom: 4px;
  }
  .p2-name {
    font-family: 'DM Sans', 'Noto Serif SC', serif;
    font-size: 13px; font-weight: 700; letter-spacing: 3px; color: var(--ink);
  }
  .p2-tag {
    font-family: 'DM Sans', 'Cormorant', serif;
    font-size: 11.5px; font-style: italic; color: var(--accent); letter-spacing: 0.3px;
  }
  /* SECTIONS */
  .sec { margin-top: 14px; }
  .sec-title {
    font-family: 'DM Sans', 'Cormorant SC', 'Cormorant', Georgia, serif;
    font-size: 12.5px; font-weight: 600; letter-spacing: 2.5px;
    color: var(--accent); text-transform: uppercase;
    display: flex; align-items: center; gap: 10px; margin-bottom: 6px;
  }
  .sec-title::after { content: ''; flex: 1; height: 0.5px; background: var(--rule); }
  /* EDUCATION */
  .edu-head { display: flex; justify-content: space-between; align-items: baseline; }
  .edu-school {
    font-family: 'DM Sans', 'Noto Serif SC', serif;
    font-size: 12px; font-weight: 700; color: var(--ink);
  }
  .edu-time {
    font-family: 'DM Sans', 'Cormorant', Georgia, serif;
    font-size: 11.5px; color: var(--muted); font-style: italic; white-space: nowrap;
  }
  .edu-sub {
    font-family: 'DM Sans', 'Cormorant', serif;
    font-size: 11.5px; font-style: italic; color: var(--mid); margin-top: 1px;
  }
  .edu-sub .gpa {
    font-style: normal; font-family: 'DM Sans', 'Noto Serif SC', serif;
    font-weight: 700; font-size: 11px; color: var(--accent);
  }
  /* SKILL TABLE */
  .sk-table { width: 100%; border-collapse: collapse; }
  .sk-table td { padding: 3px 0; font-size: 11px; line-height: 1.65; vertical-align: top; }
  .sk-table td:first-child {
    font-weight: 700; color: var(--ink); white-space: nowrap;
    padding-right: 14px; width: 80px;
  }
  .sk-table td:last-child { color: var(--mid); }
  /* ITEMS */
  .item { margin-bottom: 8px; }
  .item:last-child { margin-bottom: 0; }
  .item-head { display: flex; justify-content: space-between; align-items: baseline; gap: 8px; }
  .item-org {
    font-family: 'DM Sans', 'Noto Serif SC', serif;
    font-size: 12px; font-weight: 700; color: var(--ink);
  }
  .item-time {
    font-family: 'DM Sans', 'Cormorant', serif;
    font-size: 11.5px; color: var(--muted); font-style: italic;
    white-space: nowrap; flex-shrink: 0;
  }
  .item-sub {
    font-family: 'DM Sans', 'Cormorant', serif;
    font-size: 11.5px; font-style: italic; color: var(--mid);
    margin-bottom: 3px; line-height: 1.55;
  }
  .item-divider { width: 100%; height: 0.5px; background: var(--rule); margin: 12px 0; }
  /* BULLETS */
  .wl { list-style: none; padding-left: 0.8em; }
  .wl li {
    position: relative; padding-left: 13px;
    font-size: 11.5px; line-height: 1.78; color: var(--mid);
  }
  .wl li::before {
    content: '◆'; position: absolute; left: 0;
    font-size: 5px; top: 7px; color: var(--accent2);
  }
  .wl li strong { color: var(--ink); font-weight: 600; }
  /* RESULT MARK */
  .result-mark {
    font-family: 'DM Sans', 'Cormorant', serif;
    font-size: 11.5px; font-style: italic; font-weight: 600;
    color: var(--accent); margin-top: 3px; letter-spacing: 0.2px;
  }
  /* AWARDS */
  .award-row { display: flex; flex-wrap: wrap; gap: 6px 22px; }
  .award-tag {
    font-size: 11.5px; color: var(--mid);
    position: relative; padding-left: 13px; line-height: 1.85;
  }
  .award-tag::before {
    content: '◆'; position: absolute; left: 0;
    font-size: 5px; top: 7px; color: var(--accent2);
  }
  /* FOOTER */
  .pg-footer {
    position: absolute; bottom: 15px; left: 40px; right: 40px;
    display: flex; justify-content: space-between;
    font-family: 'DM Sans', 'Cormorant', serif;
    font-size: 10px; letter-spacing: 1.5px;
    color: var(--rule); text-transform: uppercase;
  }
  /* PAGE 2 SPACING — adjust these values to fix overflow */
  .p2 .sec            { margin-top: 20px; }
  .p2 .sec:first-child{ margin-top: 0; }
  .p2 .item           { margin-bottom: 10px; }
  .p2 .item:last-child{ margin-bottom: 0; }
  .p2 .item-divider   { margin: 14px 0; }
  .p2 .wl li          { line-height: 1.82; }
  /* PRINT */
  @media print {
    body { background: none; }
    .page { box-shadow: none; margin: 0; }
    .page:first-child { page-break-after: always; }
    .page:last-child  { page-break-after: avoid; }
  }
  @media screen and (max-width: 800px) {
    .page { width: 100%; height: auto; margin: 0; padding: 24px 22px; }
    .page::before { display: none; }
  }
</style>
</head>
<body>

<!-- PAGE 1 -->
<div class="page">
  <div class="header">
    <!-- Remove <img> tag if no photo provided -->
    <img class="avatar" src="{{照片文件名}}" alt="{{姓名}}">
    <div class="header-text">
      <div class="name">{{姓名}}</div>
      <div class="tagline">{{职位方向}} · {{学校}} {{专业}} {{学历}}</div>
      <div class="meta-row">
        <span class="meta-line"><strong>电话</strong> {{电话}} &nbsp;|&nbsp; <strong>邮箱</strong> {{邮箱}}</span>
        <span class="meta-line"><strong>GitHub</strong> <a href="{{github链接}}">{{github显示}}</a> &nbsp;|&nbsp; <strong>博客</strong> <a href="{{博客链接}}">{{博客显示}}</a></span>
      </div>
    </div>
  </div>

  <!-- 教育背景 -->
  <div class="sec">
    <div class="sec-title">教育背景</div>
    <div class="edu-head">
      <span class="edu-school">{{学校}}</span>
      <span class="edu-time">{{入学年月}} — {{毕业年月或"至今"}}</span>
    </div>
    <div class="edu-sub">{{专业}} · {{学历}} &emsp; <span class="gpa">GPA {{GPA}}</span></div>
  </div>

  <!-- 专业技能 -->
  <div class="sec">
    <div class="sec-title">专业技能</div>
    <table class="sk-table">
      <tr><td>{{技能类别1}}</td><td>{{技能描述1}}</td></tr>
      <tr><td>{{技能类别2}}</td><td>{{技能描述2}}</td></tr>
      <!-- 每行一个技能类别，通常 4-6 行 -->
    </table>
  </div>

  <!-- 实习经历（如有） -->
  <div class="sec">
    <div class="sec-title">实习经历</div>
    <div class="item">
      <div class="item-head">
        <span class="item-org">{{公司名称}}</span>
        <span class="item-time">{{开始}} — {{结束}}</span>
      </div>
      <div class="item-sub">项目：{{项目名称}} · 负责：{{负责模块}}</div>
      <div class="item-sub">技术栈：{{技术栈}}</div>
      <ul class="wl">
        <li><strong>【{{模块}}】</strong>{{STAR描述}}</li>
        <!-- 每个实习项目 3-5 条 bullet -->
      </ul>
      <div class="result-mark">▸ {{技能收获总结}}</div>
    </div>
    <!-- 多个实习项目之间加 <div class="item-divider"></div> -->
  </div>

  <!-- 项目经历（第一页放 1-2 个） -->
  <div class="sec">
    <div class="sec-title">项目经历</div>
    <div class="item">
      <div class="item-head">
        <span class="item-org">{{项目名称}}</span>
        <span class="item-time">{{开始}} — {{结束}}</span>
      </div>
      <div class="item-sub">{{项目一句话描述}}</div>
      <div class="item-sub">技术栈：{{技术栈}}</div>
      <ul class="wl">
        <li><strong>【{{模块}}】</strong>{{STAR描述}}</li>
      </ul>
      <div class="result-mark">▸ {{技能收获总结}}</div>
    </div>
  </div>

  <div class="pg-footer">
    <span>{{姓名}} · {{职位方向}}</span>
    <span>1 / 2</span>
  </div>
</div>

<!-- PAGE 2 -->
<div class="page p2">
  <div class="page2-bar">
    <span class="p2-name">{{姓名}}</span>
    <span class="p2-tag">{{职位方向}} · 求职意向：{{职位全称}}</span>
  </div>

  <!-- 项目经历（续） -->
  <div class="sec">
    <div class="sec-title">项目经历（续）</div>
    <div class="item">
      <div class="item-head">
        <span class="item-org">{{项目名称}}</span>
        <span class="item-time">{{开始}} — {{结束}}</span>
      </div>
      <div class="item-sub">{{项目一句话描述}}</div>
      <div class="item-sub">技术栈：{{技术栈}}</div>
      <ul class="wl">
        <li><strong>【{{模块}}】</strong>{{STAR描述}}</li>
      </ul>
      <div class="result-mark">▸ {{技能收获总结}}</div>
    </div>
    <div class="item-divider"></div>
    <!-- 继续添加项目 -->
  </div>

  <!-- 奖项证书 -->
  <div class="sec">
    <div class="sec-title">奖项证书</div>
    <div class="award-row">
      <span class="award-tag">{{奖项1}}</span>
      <span class="award-tag"><strong>{{重要奖项}}</strong></span>
    </div>
  </div>

  <!-- 自我评价 -->
  <div class="sec">
    <div class="sec-title">自我评价</div>
    <ul class="wl">
      <li>{{评价1}}</li>
      <li>{{评价2}}</li>
      <li>{{评价3}}</li>
    </ul>
  </div>

  <div class="pg-footer">
    <span>{{姓名}} · {{职位方向}}</span>
    <span>2 / 2</span>
  </div>
</div>

</body>
</html>
```

---

## Phase 3 — Content Distribution Strategy

**Page 1 should contain** (in order):
1. Header (name, contact)
2. 教育背景
3. 专业技能
4. 实习经历（全部）
5. 项目经历第一个（如空间允许）

**Page 2 should contain**:
1. 项目经历（续）— 剩余项目
2. 奖项证书
3. 自我评价

**How many projects per page**: Aim for page 1 content height ≈ page 2 content height. A project with 4-5 bullets takes ~130-160px. Adjust accordingly after the Playwright check.

---

## Phase 4 — Layout Verification (Playwright MCP)

**If Playwright MCP is not available**, skip to Phase 5 and note that layout has not been verified.

### Step 1: Navigate to the file

Open the HTML file with viewport width **1200px minimum** (narrower triggers mobile CSS that removes fixed height).

### Step 2: Run this exact script in the browser console

```javascript
(function() {
  const pages = document.querySelectorAll('.page');
  const out = [];
  pages.forEach((p, i) => {
    const a4 = Math.round(p.getBoundingClientRect().height);
    const orig = [p.style.height, p.style.overflow];
    p.style.height = 'auto';
    p.style.overflow = 'visible';
    const content = p.scrollHeight;
    p.style.height = orig[0];
    p.style.overflow = orig[1];
    out.push({ page: i + 1, a4, content, overflow: content - a4 });
  });
  return out;
})();
```

### Step 3: Take screenshots of each .page element

### Step 4: Interpret and fix

| Overflow value | Meaning | Fix |
|---------------|---------|-----|
| `> 50` | Content clipped, invisible to reader | Move last project item to page 2 |
| `1 to 50` | Barely clipped | Reduce `.p2 .item-divider` margin by 4-6px each, reduce `.p2 .wl li` line-height by 0.05 |
| `-30 to 0` | Perfect | Done |
| `-30 to -120` | Acceptable | Done |
| `< -120` | Too much blank space | Move first project from page 2 to page 1 |

After each fix, re-run the script. **Repeat until both pages are in the `-120 to 0` range.**

### Step 5: Verify visually

Screenshot each page and check:
- [ ] No bullet cut off at page bottom
- [ ] Page number footer visible
- [ ] Tech stack on its own line below description
- [ ] Bold tags `【...】` visible on every bullet
- [ ] Numbers look clean (DM Sans, not decorative)

---

## Phase 5 — If No Playwright Available

Estimate layout manually:

- Each bullet line ≈ 20px at 11.5px font / line-height 1.78
- A 5-bullet project item ≈ 140-160px total (header + 2 sub lines + 5 bullets + result)
- A 4-bullet project item ≈ 120-135px
- Section header + margin ≈ 30px
- A4 usable height ≈ 1050px per page (297mm minus padding, header, footer)

Add up estimates. If total > 1050px, remove one project or reduce one project from 5 bullets to 4.

Tell the user: "Layout estimated manually — open in browser to verify no overflow."

---

## Error Handling

| Situation | What to do |
|-----------|-----------|
| PDF text extraction returns garbled characters | Ask: "PDF 文字无法正确提取，请将简历内容粘贴为纯文本" |
| User provides content in a mix of languages | Write the resume in the same language as the original (Chinese resumes stay Chinese) |
| A project has no measurable result | Use qualitative bold result: `**提升代码可维护性**`, `**保障接口安全**` |
| Only one page of content | Build one page only, remove the `.p2` div entirely |
| More than two pages of content | Ask user which projects to prioritize; keep the most technically impressive and most recent |
| No photo provided | Remove the `<img class="avatar">` tag entirely from the header |
| No GitHub/blog | Remove those fields from meta-line |
| Overflow cannot be fixed without removing user-requested content | Tell the user exactly what needs to be removed and ask permission |
| Playwright script returns `null` or errors | Viewport is probably too narrow — retry at 1400px width |

---

## Final Output

Deliver:
1. The complete HTML file written to disk (filename: `{{姓名}}简历.html`)
2. A brief summary:

```
✅ Phase 1: Extracted [N] projects, [N] internships
✅ Phase 2: Rewrote [N] bullets with STAR principle  
✅ Phase 3: HTML built from template
✅ Phase 4: Layout verified — Page 1: Xpx spare, Page 2: Ypx spare
   (or: ⚠️ Layout not verified — Playwright unavailable)
```
