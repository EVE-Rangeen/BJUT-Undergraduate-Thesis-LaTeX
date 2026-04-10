# 北京工业大学本科生毕业论文 LaTeX 模板

基于 [Ziyu Zhou 的 BJUT 研究生论文模板](https://www.overleaf.com/latex/templates/bjut-undergraduate-thesis/vwdrdbfddvdd)（CC BY 4.0）修改而成，针对本科生毕业设计（论文）撰写规范进行了适配。

---

## 目录

- [环境要求](#环境要求)
- [快速开始](#快速开始)
- [修改封面信息](#修改封面信息)
- [添加新章节](#添加新章节)
- [标签与交叉引用](#标签与交叉引用)
- [双语 / 单语言支持](#双语--单语言支持)
- [编译参考文献](#编译参考文献)
- [常见缓存问题](#常见缓存问题)

---

## 环境要求

| 组件 | 说明 |
|------|------|
| **TeX 发行版** | [TeX Live](https://tug.org/texlive/)（推荐 2023 及以上） |
| **编译器** | **XeLaTeX**（必须，不可用 pdfLaTeX 或 LuaLaTeX） |
| **编辑器** | [VS Code](https://code.visualstudio.com/) + [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) 插件 |

> **注意**：所有中文字体（SimSun、SimHei、Kaiti、FangSong、Lishu）的 `.ttf/.ttc` 文件已内置在仓库根目录，无需另行安装系统字体。

---

## 快速开始

1. 克隆或下载仓库到本地：
   ```bash
   git clone https://github.com/hw401/BJUT-Undergraduate-Thesis.git
   ```
2. 用 VS Code 打开项目根目录。
3. 打开 `main.tex`，使用 LaTeX Workshop 的 **XeLaTeX** 方案编译（快捷键 `Ctrl+Alt+B`）。
4. 编译完成后，根目录下生成 `main.pdf`。

---

## 修改封面信息

封面内容直接硬编码在 `bjutthesis.cls` 中的 `\makecover` 命令里（约第 272 行），找到以下表格部分进行修改：

```latex
\makebox[30mm][s]{题目}   & \underline{\makebox[115mm][c]{论文题目第一行}} \\[22pt]
\makebox[30mm][s]{}       & \underline{\makebox[115mm][c]{论文题目第二行（如不需要留空）}} \\[22pt]
\makebox[30mm][s]{姓名}   & \underline{\makebox[115mm][c]{你的姓名}} \\[22pt]
\makebox[30mm][s]{学号}   & \underline{\makebox[115mm][c]{你的学号}} \\[22pt]
\makebox[30mm][s]{指导教师} & \underline{\makebox[115mm][c]{导师姓名}} \\[22pt]
\makebox[30mm][s]{日期}   & \underline{\makebox[115mm][c]{2025年6月1日}} \\[22pt]
```

> **提示**：如果题目只有一行，第二行的 `\makebox` 内容留空即可，下划线仍会保留。

授权声明页的日期在同一文件的 `\makestate` 命令中（约第 343 行），同样需要手动修改。

---

## 添加新章节

### 1. 新建章节文件

在根目录新建章节文件，命名规范为 `chapt2.tex`、`chapt3.tex`……以此类推。

### 2. 在主文档中引入

打开 `main.tex`，在 `\mainmatter` 和 `\include{conclusion}` 之间按顺序添加：

```latex
\mainmatter
\let\cleardoublepage\clearpage
\include{chapt1}
\let\cleardoublepage\clearpage
\include{chapt2}   % 新增章节
\let\cleardoublepage\clearpage
\include{chapt3}   % 新增章节
\let\cleardoublepage\clearpage
\include{conclusion}
```

### 3. 章节文件结构

每个章节文件以 `\bichapter` 或 `\chapter` 开头（参见[双语 / 单语言支持](#双语--单语言支持)），内部用 `\bisection` / `\section`、`\subsection`、`\subsubsection`、`\subsubsubsection` 组织层级，共支持 **4 级标题**，目录自动收录至第 2 级（`\subsection`）。

```latex
\bichapter{第二章标题}{Chapter 2 Title}\label{chap:chap2}
\linespread{1.08}\selectfont

\bisection{第一节标题}{Section Title}\label{sec:sec1}

正文内容……

\subsection{小节标题}

更细的内容……
```

---

## 标签与交叉引用

本模板使用 `cleveref` 宏包，推荐使用 `\cref{}` 进行所有引用，它会自动添加"图"、"表"、"公式"等前缀。

### 打标签

在任意浮动体或章节命令后紧跟 `\label{}`：

```latex
\bichapter{第一章}{Chapter 1}\label{chap:intro}
\bisection{背景}{Background}\label{sec:background}

\begin{figure}[htpb]
  ...
  \label{fig:architecture}
\end{figure}

\begin{table}[htpb]
  ...
  \label{tab:results}
\end{table}

\begin{equation}\label{eq:euler}
  e^{\pi i}+1 = 0
\end{equation}
```

### 引用标签

```latex
如\cref{fig:architecture}所示        % → "如图2.1所示"
详见\cref{tab:results}               % → "详见表3.2"
由\cref{eq:euler}可知                % → "由公式(1.1)可知"
见\cref{chap:intro}                  % → "见第1章"
```

### 推荐命名前缀约定

| 类型 | 前缀示例 |
|------|----------|
| 章节 | `chap:` |
| 节 | `sec:` |
| 图 | `fig:` |
| 表 | `tab:` |
| 公式 | `eq:` |
| 算法 | `alg:` |

---

## 双语 / 单语言支持

本模板提供双语标题命令，用于同时生成中文目录和英文目录（图、表目录同理）。

### 双语章节标题

```latex
% 语法：\bichapter{中文标题}{English Title}
\bichapter{系统设计}{System Design}\label{chap:design}

% 语法：\bisection{中文标题}{English Title}
\bisection{总体架构}{Overall Architecture}\label{sec:arch}
```

> 双语命令会在目录中同时显示中英文标题，并生成双语图表目录条目。

### 单语言章节标题

如果不需要英文标题，直接使用标准 LaTeX 命令：

```latex
\chapter{系统设计}\label{chap:design}
\section{总体架构}\label{sec:arch}
```

### 双语图、表题注

表格和图片支持双语题注（需要 `bicaption` 宏包，已在 `main.tex` 中引入）：

```latex
% 图片双语题注
\begin{figure}[htpb]
  \centering
  \includegraphics[width=0.8\textwidth]{example.pdf}
  \bicaption{中文图题}{English Caption}
  \label{fig:example}
\end{figure}

% 表格双语题注
\begin{table}[htpb]
  \centering
  \bicaption{中文表题}{English Caption}
  \label{tab:example}
  ...
\end{table}
```

如果只需要单语言题注，使用普通 `\caption{}` 即可。

---

## 编译参考文献

参考文献使用 **BibTeX + GBT 7714 国标格式**。

### 标准编译流程

首次编译或修改了 `reference.bib` 之后，需要按以下顺序执行 **4 步**：

```bash
xelatex main      # 第1步：生成 .aux 文件
bibtex main       # 第2步：处理参考文献
xelatex main      # 第3步：将文献列表写入 PDF
xelatex main      # 第4步：修正所有交叉引用编号
```

> 使用 LaTeX Workshop 的 `latexmk` 方案可自动完成上述 4 步。

### 在正文中引用文献

```latex
% 上标引用（最常用）
RSA\cite{rsa}算法

% 正文内引用
文献\citen{rsa}提出了……
```

### 添加文献条目

直接编辑 `reference.bib` 文件，每条文献格式如下：

```bibtex
@article{key2024,
  author  = {作者姓名},
  title   = {文章标题},
  journal = {期刊名称},
  year    = {2024},
  volume  = {1},
  pages   = {1--10},
}

@inproceedings{key2024b,
  author    = {Author Name},
  title     = {Paper Title},
  booktitle = {Conference Name},
  year      = {2024},
}
```

> `\nocite{*}` 已在 `main.tex` 中启用，所有在 `reference.bib` 中定义的文献均会出现在参考文献列表，无论是否在正文中 `\cite`。如需只列出已引用的文献，删除该行即可。

---

## 常见缓存问题

LaTeX 编译会产生大量中间缓存文件，有时旧的缓存会导致编译错误或输出不正确。

### 应被 `.gitignore` 忽略的文件

以下文件是编译产物，不应提交到版本控制：

```
*.aux  *.log  *.out  *.toc  *.nlo  *.synctex.gz  *.bbl  *.blg
```

### 手动清除缓存

删除根目录下所有缓存文件：

```powershell
# PowerShell
Remove-Item *.aux, *.log, *.out, *.toc, *.nlo, *.synctex.gz, *.bbl, *.blg -ErrorAction SilentlyContinue
```

清除后需重新走完[标准编译流程](#标准编译流程)（4 步）。

### 修复已被 git 追踪的缓存文件

如果缓存文件已经被 git 追踪（出现在 `git status` 中），需要将其从追踪中移除：

```bash
# 移除单个文件
git rm --cached main.aux

# 移除所有已追踪的缓存文件并重建索引
git rm -r --cached .
git add .
git commit -m "清理gitignore缓存"
```

### 常见报错与解决

| 报错 / 现象 | 原因 | 解决方法 |
|-------------|------|----------|
| 参考文献编号显示为 `[?]` | 未运行 bibtex 或编译次数不足 | 按标准 4 步重新编译 |
| 目录页码错误 | `.toc` 文件过期 | 删除 `.toc` 后重新编译两次 |
| 交叉引用显示 `??` | `.aux` 文件缺失或过期 | 删除所有 `.aux` 后重新编译两次 |
| 字体找不到 | 编译器不是 XeLaTeX | 确认使用 XeLaTeX 编译 |
| `\bichapter` 未定义 | 编译了错误的入口文件 | 确认编译的是 `main.tex` 而非章节文件 |

---

## Acknowledgements

This project includes content adapted from:
- **Author**: Ziyu Zhou
- **Source**: https://www.overleaf.com/latex/templates/bjut-undergraduate-thesis/vwdrdbfddvdd
- **License**: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/)
