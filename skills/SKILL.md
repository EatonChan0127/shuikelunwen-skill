---
name: course-paper-gb7714
description: Generates course papers and reports from a user-supplied title and requirements, performs web research for real citable sources, writes LaTeX with GB/T 7714-2015 references and Chinese typography defaults, then exports Word (.docx) without manual user steps. Use when the user asks for 水课论文, 课程论文, 课程报告, 结课论文, literature-based reports with real citations, LaTeX then Word, or GB/T 7714-2015 formatting. Same workflow applies in Cursor, VS Code–based agents, Claude Code, OpenAI Codex, or Trae IDE when this file is in context or linked from project rules or instructions.
---

# Course paper (LaTeX → Word, GB/T 7714-2015) / 课程论文（LaTeX → Word，GB/T 7714-2015）

**中文摘要**：本技能规定代理如何根据题目与要求联网检索**真实可引用文献**，先写 **`paper.tex` + `references.bib`**（国标 GB/T 7714-2015 顺序编码），再编译并导出 **Word**，全程由代理执行命令，用户无需手工排版。

**English summary**: This skill tells the agent how to research **verifiable sources**, author **`paper.tex` + `references.bib`** (GB/T 7714-2015 numeric), compile, export **Word**, and run commands end-to-end without asking the user to hand-format.

---

## Agent environments (Cursor, VS Code, Claude Code, Codex, Trae) / 运行环境

**English**: These instructions are **tool-agnostic**: any agent that can read repo files, search the web, and run shell commands can follow them. Wire the repo so the agent actually loads this file.

**中文**：以下规则与具体产品无关；任何能读仓库、联网搜索、跑终端命令的代理都应遵守。请通过下表在各工具中**显式挂载**本文件。

| Tool | Suggested wiring |
|------|------------------|
| **Cursor** | Keep this directory under `.cursor/skills/course-paper-gb7714/` so Skills discovery applies; optional `.cursor/rules` entry pointing at `SKILL.md`. |
| **VS Code** (Copilot Chat / inline agent) | Add repo instructions in `.github/copilot-instructions.md` (or user **GitHub Copilot: Instructions**) with one line: follow `.cursor/skills/course-paper-gb7714/SKILL.md` for 课程论文 tasks. Attach `SKILL.md` in chat when the UI supports `@` files. |
| **Claude Code** | In `CLAUDE.md` (repo root): e.g. “For 课程论文 / GB7714 / LaTeX→Word, follow `.cursor/skills/course-paper-gb7714/SKILL.md`.” Or `@.cursor/skills/course-paper-gb7714/SKILL.md` at session start. |
| **Codex** (OpenAI extension / CLI) | Put the same path in Codex **custom instructions** / `AGENTS.md`, or open with: “Read and strictly follow `.cursor/skills/course-paper-gb7714/SKILL.md`.” |
| **Trae IDE** | In **Settings → Rules** (AI panel), edit **project** `project_rules.md` (often `.trae/project_rules.md`) to require following `.cursor/skills/course-paper-gb7714/SKILL.md` for course-paper tasks; optional thin skill under `.trae/skills/` pointing at the same file. See [TUTORIAL.md](TUTORIAL.md) §2.5. |

**中文**：人类用户的逐步说明与复制用提示词见 [TUTORIAL.md](TUTORIAL.md)（含 **Trae** §2.5）。

**English**: Human-facing setup and copy-paste prompts: [TUTORIAL.md](TUTORIAL.md).

---

## When to use / 何时使用

**English**: Apply when the user wants an automated course paper/report with **real literature**, **correct citations**, **LaTeX as the canonical draft**, and **final delivery as Microsoft Word** (e.g. 水课论文, 课程论文/报告, 文献综述, GB/T 7714).

**中文**：用户要**自动撰写课程/结课论文或报告**、**真实文献与规范引用**、**以 LaTeX 为主稿**且**最终交 Word** 时启用（含 水课论文、国标参考文献等说法）。

---

## Inputs to collect (minimal) / 需向用户确认的最小信息

**English**:

1. **Title** of the paper/report (Chinese or English).
2. **Assignment requirements**: length, sections, rubric hints, scope, forbidden topics, language (default: Chinese).
3. **Optional format overrides** (if absent, use “Typography defaults”).
4. **Output paths** (defaults): `paper/paper.tex`, `paper/references.bib`, `paper/build/paper.docx` (adjust per project).

If requirements are vague, infer a reasonable structure (摘要/引言/正文/结论/参考文献) and state assumptions briefly.

**中文**：

1. **题目**（中或英）。  
2. **作业要求**：字数、章节、评分侧重点、范围、禁写内容、语言（默认中文）。  
3. **版式覆盖**（若未提供则用下文默认）。  
4. **输出路径**（默认可用 `paper/` 下上述文件）。

要求模糊时，代理应合理推断结构并在对话中简要说明假设。

---

## Research and citation rules / 检索与引用规则

**English**:

1. Prefer **verifiable** sources: peer-reviewed work, standards, official pages, publisher pages with DOI. Avoid anonymous forums as primary evidence.
2. Use **web search** (and fetch when helpful) for title, authors, year, venue, **DOI or stable URL**, and page numbers when needed.
3. Build **`references.bib`** with accurate BibTeX fields—**never invent metadata**; if unsure, search again or drop the source.
4. Non–common-knowledge claims should cite a real `references.bib` key.
5. Citation style: GB/T 7714-2015 **numeric** via `biblatex-gb7714` (`gb7714-numerical`), unless the user requests author–year.

**中文**：

1. 优先可核对来源（期刊、标准、政府/高校、带 DOI 的出版页等），避免以匿名论坛为主要依据。  
2. 必须**联网检索**（必要时抓取页面）补齐题名、作者、年份、出处、**DOI 或稳定 URL**、起止页等。  
3. **`references.bib` 字段须真实**；不确定则再搜或不用该条。  
4. 非常识性陈述应有对应引用。  
5. 参考文献体例：**GB/T 7714-2015 顺序编码**（`biblatex-gb7714`，`gb7714-numerical`）；仅当用户明确要求时使用著者–出版年。

---

## Typography defaults (when user gives no format) / 版式默认（用户未指定时）

**English**: Songti-class body, black text, Chinese type sizes as below, 1.5 line spacing for typical school reports unless the user overrides.

**中文**：宋体、黑色；字号按中文习惯近似；行距默认 1.5（可按用户改）。

| Element | Spec |
|--------|------|
| Body font | 宋体 Songti: Windows `SimSun`; macOS `Songti SC`; Linux `Noto Serif CJK SC` |
| Colour | Black (`\color{black}`) |
| Body size | 小四 ≈ 12 pt |
| Subheadings | 四号 ≈ 14 pt |
| Paper title | 三号 ≈ 16 pt |
| Line spacing | 1.5 (typical; switch if user requires) |
| References | GB/T 7714-2015 numeric (`biblatex-gb7714`, `gb7714-numerical`) |

---

## LaTeX stack (canonical artifact) / LaTeX 技术栈（主稿）

**English**: XeLaTeX + `ctexart`/`ctexrep`; `biblatex` + `gb7714-numerical`. Keep figures/tables only if required.

**中文**：引擎 **XeLaTeX**；文档类 **`ctexart`**（长报告可用 `ctexrep`）；参考文献 **`biblatex` + `gb7714-numerical`**。图表仅在有要求时加入。

```latex
\documentclass[UTF8,a4paper,zihao=-4]{ctexart}
\usepackage{geometry}
\geometry{margin=2.5cm}
\usepackage{xcolor}
\usepackage[backend=biber,style=gb7714-numerical]{biblatex}
\addbibresource{references.bib}
\begin{document}
\title{\zihao{3}\textbf{论文题目 Paper title}}
\maketitle
\section{\zihao{4} 一级标题 Section}
正文 Body\cite{example2023}。
\printbibliography
\end{document}
```

---

## Build pipeline (agent runs end-to-end) / 构建流水线（代理全程执行）

**English**: From the directory containing `paper.tex`. Install TeX Live/MiKTeX, Pandoc, and Biber if missing.

**中文**：在 `paper.tex` 所在目录执行；若本机缺 TeX / Pandoc / Biber，由代理提示或按环境安装。

1. **Validate PDF (recommended)** / **建议先编译 PDF 校验引用**  

```bash
latexmk -xelatex -pdf -interaction=nonstopmode -halt-on-error paper.tex
```

2. **Produce Word (primary)** / **导出 Word（首选）**  

```bash
pandoc paper.tex -o paper.docx --from=latex --citeproc
```

**English**: If Pandoc’s LaTeX reader drops `biblatex`, emit **`paper.md`** with Pandoc cites `[@key]` and YAML pointing at the same `references.bib`, then:

**中文**：若 Pandoc 解析 LaTeX 丢 `biblatex`，则生成 **`paper.md`**（`[@key]` + YAML 指向同一 `references.bib`），再：

```yaml
---
title: "论文题目"
bibliography: references.bib
csl: https://www.zotero.org/styles/china-national-standard-gb-t-7714-2015-numeric
---
```

```bash
pandoc paper.md -o paper.docx --citeproc
```

Use a **local** `.csl` if the network is unreliable. / **中文**：网络不稳时在仓库放本地 `.csl`。

3. **Deliverables** / **交付物**: `paper.tex`, `references.bib`, `paper.pdf` (if built), `paper.docx`.

---

## Quality checks before hand-off / 交付前检查

**English**: Bib keys exist; no “TODO” placeholders; GB numeric style and 参考文献 heading; Word readable with Songti-class body where possible.

**中文**：引用键与 `.bib` 一致；无「待补充」式占位；国标顺序制与「参考文献」标题；Word 打开正常，宋体类字体按系统说明。

---

## Anti-patterns / 禁止事项

**English**: No fabricated DOIs/ISBNs/journals/pages; never skip writing `.tex` first; no scraping paywalled full text without legal access.

**中文**：禁止编造 DOI、书号、期刊名、页码；禁止跳过 LaTeX 主稿；无合法权限不抓取付费全文。

---

## Optional expansion / 可选扩展

**English**: Split into `sections/*.tex` with `\input{}`; keep one `references.bib`.

**中文**：长文可拆 `sections/*.tex` 用 `\input{}`；参考文献仍建议单文件 `references.bib`。

---

## Additional resources / 更多资料

- [TUTORIAL.md](TUTORIAL.md) — setup & prompts / 教程与提问模板  
- [reference.md](reference.md) — LaTeX / Pandoc snippets / 模板与命令  
