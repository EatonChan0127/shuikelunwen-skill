# 课程论文技能教程 · Course-paper skill tutorial

**适用工具 · Tools**: Cursor / VS Code / Claude Code / Codex / Trae IDE

---

## 概述 · Overview

**中文**：说明如何在不同编辑器与 AI 助手中使用 **course-paper-gb7714**：按题目与要求联网检索真实文献 → 写入 LaTeX（`.tex` + `.bib`）→ 编译校验 → 导出 Word（`.docx`）。参考文献默认 **GB/T 7714-2015** 顺序编码；正文默认版式见 `SKILL.md` 的「Typography defaults」。

**English**: This guide explains how to use the **course-paper-gb7714** workflow in different editors and coding agents: collect a title and requirements → web research for verifiable sources → write LaTeX (`.tex` + `.bib`) → compile → export Word (`.docx`). Bibliography defaults to **GB/T 7714-2015** numeric style; body typography defaults are in `SKILL.md` under “Typography defaults”.

**技能路径 · Skill paths** (relative to repo root):

```text
.cursor/skills/course-paper-gb7714/
├── SKILL.md       # Agent rules (required) / AI 执行规范（必读）
├── TUTORIAL.md    # Human guide / 人类教程
└── reference.md   # LaTeX / Pandoc cheat sheet / 模板与命令速查
```

---

## 1. 环境准备 · Prerequisites (one-time)

**中文**：AI 会在本机执行命令，需要安装 TeX（含 `xelatex`、`latexmk`、`biber`）、Pandoc，以及通常随 TeX 自带的 `biblatex-gb7714`。Windows 常用 [MiKTeX](https://miktex.org/) 或 [TeX Live](https://tug.org/texlive/)；Pandoc 见 [pandoc.org](https://pandoc.org/installing.html)。缺包时用发行版包管理器安装 `biblatex-gb7714`。

**English**: The agent runs commands locally. Install a TeX distribution (with `xelatex`, `latexmk`, `biber`), Pandoc, and usually `biblatex-gb7714` via the TeX package manager if compilation complains about missing files.

| 组件 · Component | 作用 · Role |
|------------------|-------------|
| TeX 发行版 · TeX distro | `xelatex`, `latexmk`, `biber` |
| Pandoc | `.tex` / `.md` → `.docx` |
| biblatex-gb7714 | GB/T 7714–style bibliography |

**自检 · Self-check** (PowerShell):

```powershell
xelatex --version
latexmk -v
pandoc --version
biber --version
```

**中文**：若某条命令不存在，先安装再让 AI 生成论文。

**English**: Fix any missing command before asking the agent to build the paper.

---

## 2. 各工具接线 · Wiring the skill per tool

**核心原则 · Core rule**: make sure the active conversation loads **`SKILL.md`**.

### 2.1 Cursor

**中文**：保留 `.cursor/skills/course-paper-gb7714/SKILL.md`；在 Cursor 设置中确认项目 Skills 生效。对话可直接描述需求，或 `@.cursor/skills/course-paper-gb7714/SKILL.md` 再贴题目与要求。可在 `.cursor/rules` 中写明课程论文任务遵守该 `SKILL.md`。

**English**: Keep the skill under `.cursor/skills/…`; confirm project skills are enabled. Mention the skill in natural language or `@` the `SKILL.md` path. Optional `.cursor/rules` pointer for course-paper tasks.

### 2.2 VS Code (GitHub Copilot Chat, etc.)

**中文**：Copilot 没有与 Cursor 完全相同的 Skills 注入；建议在仓库根目录 `.github/copilot-instructions.md` 追加一段，指向 `SKILL.md`；或在对话首条要求阅读该文件。

**English**: Add a short pointer in `.github/copilot-instructions.md` (or Copilot user instructions) so the agent follows `.cursor/skills/course-paper-gb7714/SKILL.md` for course-paper / GB7714 / LaTeX→Word tasks. Attach `SKILL.md` in chat when supported.

**示例片段 · Snippet** (Chinese prompt file):

```markdown
## 课程论文（LaTeX → Word，GB/T 7714-2015）

当用户要求课程论文、结课报告、文献综述、GB7714、或 LaTeX 转 Word 时：
严格按仓库内 `.cursor/skills/course-paper-gb7714/SKILL.md` 执行；先写 `paper.tex` 与 `references.bib`，再编译并导出 `paper.docx`。
```

### 2.3 Claude Code

**中文**：在仓库根 `CLAUDE.md` 中加入：课程论文/报告遵循 `.cursor/skills/course-paper-gb7714/SKILL.md`。新会话可 `@` 该文件后写题目与要求。

**English**: In root `CLAUDE.md`, state that course papers must follow `.cursor/skills/course-paper-gb7714/SKILL.md`. Start sessions with `@` that file when needed.

### 2.4 OpenAI Codex (extension / CLI)

**中文**：在 Codex **Custom instructions** 或项目 `AGENTS.md`（视版本而定）中加入对 `SKILL.md` 的强制引用。

**English**: In Codex custom instructions or `AGENTS.md`, require reading and following `.cursor/skills/course-paper-gb7714/SKILL.md` end-to-end for Chinese course papers, Word output, or GB/T 7714 references.

```markdown
When the user requests a Chinese course paper, final report as Word, or GB/T 7714 references:
read and follow the repository file `.cursor/skills/course-paper-gb7714/SKILL.md` end-to-end.
```

### 2.5 Trae IDE

**中文**：[Trae](https://www.trae.ai/) 是基于 Code-OSS 的 AI IDE，通过 **Trae Rules（规则）** 约束 Agent 行为；较新版本还支持 **`.trae/rules/`**、**`.trae/skills/`**（分步技能）以及项目级 **MCP**（`.trae/mcp.json`）。课程论文技能与 Cursor 类似：要让 Agent **读到本仓库的 `SKILL.md`**。

**English**: [Trae](https://www.trae.ai/) is an AI IDE (Code-OSS–based). It uses **Trae Rules** (Markdown) to constrain agents; newer versions also support **`.trae/rules/`**, **`.trae/skills/`** (procedural skills), and project **MCP** (`.trae/mcp.json`). Wire this repo so the agent loads **`SKILL.md`**—same intent as Cursor.

**接线方式 · Wiring (pick one or combine)**：

1. **项目规则 · Project rules（推荐 · recommended）**  
   - **中文**：在 Trae 中打开本项目 → AI 对话窗口右上角 **设置 → Rules（规则）** → **项目规则** → 创建或编辑 **`project_rules.md`**（部分版本会在项目下生成 **`.trae/project_rules.md`**）。写入下方「示例片段」，要求凡涉及课程论文、GB/T 7714、LaTeX→Word 时**必须**执行 `.cursor/skills/course-paper-gb7714/SKILL.md`。  
   - **English**: Open the project in Trae → **Settings → Rules** in the AI panel → **Project rules** → create/edit **`project_rules.md`** (often at **`.trae/project_rules.md`**). Paste the snippet below so any course-paper / GB7714 / LaTeX→Word task follows **`SKILL.md`**.

2. **个人规则 · User rules（全局 · global）**  
   - **中文**：同一入口下的 **个人规则** / **`user_rules.md`**（多位于用户目录 **`~/.trae/user_rules.md`**）可写通用偏好；**项目规则优先于个人规则**（与官方说明一致时）。  
   - **English**: **User rules** (`user_rules.md`, often `~/.trae/user_rules.md`) set global habits; **project rules override** user rules when they conflict.

3. **Trae Skills 目录 · `.trae/skills/`（可选 · optional）**  
   - **中文**：若你使用的 Trae 版本会从 **`.trae/skills/`** 按需加载技能，可在该目录下新建 **`course-paper-gb7714/SKILL.md`**，内容仅为**指向**本仓库权威副本，避免两处长期重复维护，例如：「执行课程论文任务时，以项目内 `.cursor/skills/course-paper-gb7714/SKILL.md` 为准。」  
   - **English**: If your Trae build loads skills from **`.trae/skills/`**, add **`course-paper-gb7714/SKILL.md`** there as a **thin pointer** to the canonical `.cursor/skills/course-paper-gb7714/SKILL.md` to avoid duplicating the full spec.

4. **对话中显式引用 · Explicit @ in chat**  
   - **中文**：与 Cursor 类似，在新任务中 **@ 工作区文件** 选中 **`SKILL.md`**，再粘贴题目与要求。  
   - **English**: Like Cursor, **`@`** the workspace file **`SKILL.md`**, then paste the assignment.

**示例片段 · Snippet for `project_rules.md` / `.trae/project_rules.md`**：

```markdown
## Course paper / 课程论文（LaTeX → Word，GB/T 7714-2015）

When the user asks for a course paper, final report, literature review, 水课论文, GB/T 7714, or LaTeX→Word export:
- Read and follow **`.cursor/skills/course-paper-gb7714/SKILL.md`** end-to-end.
- Produce `paper.tex`, `references.bib`, compile with XeLaTeX + biber, then export `paper.docx`; do not invent DOIs.

当用户要求课程论文、结课报告、文献综述、水课论文、国标参考文献或 LaTeX 转 Word 时：
- 全文遵守项目内 **`.cursor/skills/course-paper-gb7714/SKILL.md`**；
- 先写 `paper.tex` 与 `references.bib`，再编译并导出 `paper.docx`；不得编造 DOI。
```

**版本差异提示 · Version note**：Trae 各版本对 Rules / Skills 路径的 UI 可能略有不同；若找不到「创建 `project_rules.md`」，可在项目根新建 **`.trae/project_rules.md`** 并粘贴上段，或在官方文档中搜索「Trae Rules」「项目规则」。

---

## 3. 推荐提问模板 · Prompt template

**中文**：复制后替换括号内容。

**English**: Copy and replace bracketed fields.

```text
请按仓库 .cursor/skills/SKILL.md 执行。
Follow .cursor/skills/SKILL.md strictly.

【题目 Title】：
【课程/教师要求 Assignment】：（字数、章节；无则写「无，用 SKILL 默认版式」 / or “none—use SKILL defaults”）
【语言 Language】：中文 / English
【输出目录 Output dir】：默认 paper/ 或 ______

要求 / Requirements: 联网检索可核实文献，references.bib 不得编造 DOI；先 paper.tex + references.bib，再 latexmk，最后 paper.docx；列出最终路径。
```

---

## 4. 产出文件 · Expected artifacts

**中文**：主稿、文献库、可选 PDF、提交用 Word；若 LaTeX→docx 丢引用，按 `SKILL.md` 用 `paper.md` + CSL 兜底。

**English**: Main `.tex`, `references.bib`, optional PDF for proofing, final `.docx`; use the `paper.md` + `--citeproc` fallback from `SKILL.md` if needed.

| 文件 · File | 说明 · Note |
|-------------|-------------|
| `paper/paper.tex` (or agreed path) | Main manuscript / 主稿 |
| `references.bib` | Metadata for `\cite{}` / `[@key]` |
| `paper.pdf` | Optional compile check / 可选编译检查 |
| `paper.docx` | Submission target / 提交用 Word |

**中文**：离线国标 CSL 可从 [Zotero Styles](https://www.zotero.org/styles) 搜索 `GB/T 7714-2015` numeric，保存为 `gb-t-7714-2015-numeric.csl` 与文稿同目录。详见 `reference.md`。

**English**: Download a GB/T 7714-2015 numeric `.csl` from the Zotero style repository for offline Pandoc. See `reference.md`.

---

## 5. 常见问题 · FAQ

**Q（中）**：编译缺 `biblatex-gb7714` 或 biber 失败？  
**Q (EN)**: Missing `biblatex-gb7714` or biber errors?

**A**：用包管理器安装 `biblatex-gb7714`；使用 `latexmk -xelatex` 且 `backend=biber`。  
**A (EN)**: Install the package; use XeLaTeX + Biber as in `SKILL.md`.

---

**Q（中）**：Word 里不是宋体？  
**Q (EN)**: Songti not showing in Word?

**A**：Word 用本机字体；Windows `SimSun`，macOS `Songti SC`，Linux 建议 Noto Serif CJK SC 并改 `\setCJKmainfont`。  
**A (EN)**: Word uses installed OS fonts; adjust `\setCJKmainfont` per platform.

---

**Q（中）**：Pandoc 从 `.tex` 转 `.docx` 丢参考文献？  
**Q (EN)**: Pandoc drops references from `.tex`?

**A**：按 `SKILL.md` fallback：生成 `paper.md` + 同一 `references.bib` + `--citeproc`。  
**A (EN)**: Use the `paper.md` + `--citeproc` fallback in `SKILL.md`.

---

**Q（中）**：只用 Cursor 或只用 Trae 可以吗？  
**Q (EN)**: Cursor or Trae only?

**A**：可以；其他工具说明是为了同一 `SKILL.md` 在 VS Code / Claude Code / Codex / Trae 也可用。  
**A (EN)**: Yes; other sections exist so the same `SKILL.md` works across tools including Trae.

---

## 6. 相关链接 · Related files

- [SKILL.md](SKILL.md) — agent rules / AI 执行规范  
- [reference.md](reference.md) — templates & commands / 模板与命令  
