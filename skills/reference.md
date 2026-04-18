# Reference: templates and tooling / 参考：模板与命令

**English**: LaTeX / Pandoc snippets and one-liners. For setup and per-tool wiring (including **Trae IDE**, TUTORIAL §2.5), read [TUTORIAL.md](TUTORIAL.md).

**中文**：LaTeX / Pandoc 片段与一行命令。安装与各工具接线（含 **Trae**，见 `TUTORIAL.md` §2.5）见 [TUTORIAL.md](TUTORIAL.md)。

---

## Full XeLaTeX skeleton (`paper.tex`) / 完整 XeLaTeX 骨架

**English**: Point `\setCJKmainfont` at a Songti-class font installed on the OS (see SKILL.md typography defaults).

**中文**：将 `\setCJKmainfont` 指向本机已安装的宋体类字体（版式默认见 `SKILL.md`）。

```latex
\documentclass[UTF8,a4paper,zihao=-4]{ctexart}
\usepackage{geometry}
\geometry{a4paper, margin=2.5cm}
\usepackage{xcolor}
\usepackage{setspace}
\onehalfspacing
\usepackage{hyperref}
\hypersetup{colorlinks=true,linkcolor=black,citecolor=black,urlcolor=black}
\usepackage[backend=biber,style=gb7714-numerical]{biblatex}
\addbibresource{references.bib}

% Songti (pick one that exists) / 宋体（择一可用）
\setCJKmainfont{SimSun}[AutoFakeBold=true] % Windows 宋体

\title{\zihao{3}\textbf{课程论文题目}}
\author{姓名}
\date{\today}

\begin{document}
\maketitle
\begin{abstract}\zihao{-4}
摘要内容。
\end{abstract}
\section{\zihao{4} 引言}
正文段落\cite{demo2022}。
\section{\zihao{4} 分析与讨论}
\subsection{\zihao{-4} 小节标题仍用正文号或按要求}
内容。
\section{\zihao{4} 结论}
内容。
\printbibliography[title={参考文献}]
\end{document}
```

---

## Example `references.bib` / 示例参考文献库

**English**: Demo entry only—replace with verified metadata from your literature search.

**中文**：仅为示例条目；写作时请换成检索核对后的真实字段。

```bibtex
@article{demo2022,
  title   = {示例题名},
  author  = {张三 and 李四},
  journal = {某某学报},
  year    = {2022},
  volume  = {10},
  number  = {3},
  pages   = {12--20},
  doi     = {10.0000/xyz.2022.0001}
}
```

---

## Pandoc markdown fallback (`paper.md`) / Pandoc Markdown 兜底

**English**: Use when Pandoc’s LaTeX reader drops `biblatex` blocks; keep the same `references.bib` keys as in `.tex`.

**中文**：当 Pandoc 读 LaTeX 丢参考文献时使用；`.bib` 键需与 `.tex` 中一致。

```yaml
---
title: "课程论文题目"
author: "姓名"
lang: zh-CN
linestretch: 1.5
bibliography: references.bib
csl: gb-t-7714-2015-numeric.csl
reference-section-title: "参考文献"
---

## 引言

正文[@demo2022]。

## 结论

……
```

**English**: Place `gb-t-7714-2015-numeric.csl` next to the file (from the Zotero style repository) for offline `--citeproc`.

**中文**：离线使用时把 `gb-t-7714-2015-numeric.csl` 放在同目录（自 Zotero 样式库下载）。

---

## Windows PowerShell one-liner / Windows 一行命令

**English**: Run inside the folder that contains `paper.tex`.

**中文**：在包含 `paper.tex` 的目录下执行。

```powershell
latexmk -xelatex -pdf -interaction=nonstopmode paper.tex; pandoc paper.tex -o paper.docx --from=latex --citeproc
```

**English**: If the Pandoc step fails, run Pandoc on `paper.md` as described in `SKILL.md`.

**中文**：若 Pandoc 步骤失败，按 `SKILL.md` 对 `paper.md` 再导出。
