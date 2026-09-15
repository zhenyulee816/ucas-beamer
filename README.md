# UCAS Minimal Beamer

这是一个独立、极简、可编译的中国科学院大学 UCAS Beamer 模板工程。它不是科研汇报 demo，而是 `college-beamer` demo 的 UCAS 视觉版本：页面结构、演示目的和内容顺序贴近参考 PDF，视觉资产替换为 UCAS 素材，整体保持白底、大留白、左上角 logo block、克制的深色章节页和主色代码块。

## 文件结构

```text
ucas-minimal-beamer/
├─ main.tex
├─ main-zh.tex
├─ ucasminimal.sty
├─ latexmkrc
├─ README.md
├─ assets/
│  ├─ ucas-horizontal-logo-blue.png
│  ├─ ucas-horizontal-logo-white.png
│  ├─ ucas-emblem-blue.png
│  ├─ ucas-emblem-white.png
│  └─ ucas-background.png
└─ tools/
   └─ install-source-han-serif-windows.ps1
```

`main.tex` 是英文 demo，`main-zh.tex` 是中文预览 demo。二者使用同一个样式文件 `ucasminimal.sty`。

## 编译方式

英文 demo：

```bash
latexmk -xelatex main.tex
```

中文预览：

```bash
latexmk -xelatex main-zh.tex
```

如果没有 `latexmk`，可以连续运行两次 XeLaTeX：

```bash
xelatex main.tex
xelatex main.tex
```

中文预览对应为：

```bash
xelatex main-zh.tex
xelatex main-zh.tex
```

必须使用 XeLaTeX。目录、章节页和交叉引用需要至少两轮编译才能稳定。如果 VS Code 或 PDF 阅读器占用了 `main.pdf`，可能会出现 `Unable to open "main.pdf"`，关闭 PDF 预览后重新编译即可。

## 如何用这套模板做 Presentation

推荐工作流是：把 `main.tex` 当作结构示例，复制一份作为你的正式报告文件，然后只改内容，不动主题样式。

例如：

```text
main.tex          保留为英文 demo
main-zh.tex       保留为中文预览
my-talk.tex       你的正式 presentation
ucasminimal.sty   主题样式，除非改视觉系统，否则不要频繁改
```

新建 `my-talk.tex` 时保留最小导言区：

```tex
% !TEX program = xelatex
\documentclass[aspectratio=169]{beamer}
\usepackage{ucasminimal}

\title{你的报告标题}
\subtitle{你的副标题}
\date{2026 年 7 月}

\begin{document}
\maketitle

% slides here

\QApage
\end{document}
```

### 组织一份报告

一份完整 presentation 通常可以按下面顺序组织：

```tex
\maketitle

\begin{frame}{说明}
  这里写报告背景、许可说明、任务说明或开场说明。
\end{frame}

\section{Introduction}

\begin{frame}{Motivation}
  ...
\end{frame}

\section{Method}

\begin{frame}{Core Idea}
  ...
\end{frame}

\section{Summary}

\begin{frame}{Takeaways}
  ...
\end{frame}

\QApage
```

每写一个 `\section{...}`，模板会自动插入一页深色章节目录页。普通内容页保持白底、左上角 logo block 和左对齐标题。

### 写普通内容页

最常用的页面是 `frame`：

```tex
\begin{frame}{Slide Title}
  \begin{itemize}
    \item 第一条要点
    \item 第二条要点
    \item 第三条要点
  \end{itemize}
\end{frame}
```

如果想让要点逐条出现，可以使用 Beamer overlay：

```tex
\begin{frame}{Step-by-step}
  \begin{itemize}[<+->]
    \item First point
    \item Second point
    \item Third point
  \end{itemize}
\end{frame}
```

正文建议保持短句和少量 bullet。这个模板的视觉核心是留白和节奏，不适合把一页塞成论文正文。

### 使用分栏

分栏适合放“左文字、右图表”或“左定义、右示例”：

```tex
\begin{frame}{Two-column Layout}
  \begin{columns}[T]
    \begin{column}{0.52\textwidth}
      \begin{itemize}
        \item 左侧写主要解释
        \item 控制文字长度
      \end{itemize}
    \end{column}
    \begin{column}{0.40\textwidth}
      \includegraphics[width=\textwidth]{assets/example.png}
    \end{column}
  \end{columns}
\end{frame}
```

建议每页最多两栏。三栏以上会削弱这个模板的极简感。

### 添加图片

建议把报告图片放在 `assets/` 或自建的 `figures/` 目录中：

```text
assets/
├─ ucas-background.png
└─ figures/
   └─ result.png
```

在页面中使用：

```tex
\begin{frame}{Adding Images}
  \includegraphics[width=0.82\textwidth]{assets/figures/result.png}
\end{frame}
```

图片路径使用相对路径，编译时从 `.tex` 文件所在目录开始解析。

### 添加公式

数学公式保持 LaTeX 默认风格即可：

```tex
\begin{frame}{An Equation}
  \[
    i\hbar\frac{\partial}{\partial t}\Psi(\mathbf r,t)
    = \hat H \Psi(\mathbf r,t)
  \]
\end{frame}
```

不要额外引入复杂数学字体包，除非你明确知道兼容性影响。

### 使用代码块

模板提供了手写行号代码块 `ucascodebox`，适合展示短代码片段：

```tex
\begin{ucascodebox}{Minimum Beamer Document}
  \ucascodeline{1}{\codebs documentclass\{beamer\}}
  \ucascodeline{2}{\codebs begin\{document\}}
  \ucascodeline{3}{\codebs begin\{frame\}\{Hello, world!\}}
  \ucascodeline{4}{\codebs end\{frame\}}
  \ucascodeline{5}{\codebs end\{document\}}
\end{ucascodebox}
```

行号颜色由 `UCASCodeNumber` 控制，默认从主色 `UCASBlue` 混出浅色调：

```tex
\colorlet{UCASCodeNumber}{white!56!UCASBlue}
```

因此如果你改变主色，代码行号会自动跟着变成同色系的浅色。较长代码也可以使用 `ucascode` 环境，它基于 `listings`，不需要 `minted` 或 `shell-escape`。

### 使用 block

普通 block 适合强调定义、结论或简短提示：

```tex
\begin{frame}{A Block}
  \begin{block}{Key Message}
    这里写一段需要强调的内容。
  \end{block}
\end{frame}
```

不要把每个内容都做成 block。这个模板不是彩色卡片式 PPT，block 应该少用、克制使用。

### 结束页

最终页使用：

```tex
\QApage
```

它会生成极简 Q&A 页，显示：

```text
Q&A
感谢您的聆听和反馈
```

## 修改标题与日期

在 `.tex` 文件开头修改：

```tex
\title{College Beamer\\Presentation Themes}
\subtitle{Using \LaTeX{} to prepare slides}
\date{Created 22 May 2022\\Updated 20 Sep 2024}
```

如需设置页脚短标题，可在 `\begin{document}` 前加入：

```tex
\ucasShortTitle{Short Title}
```

这些命令都有默认空值，不设置也可以正常编译。

## 替换 logo 与背景

默认图片路径指向 `assets/`。如果要替换素材，可以直接覆盖同名文件，也可以在导言区设置：

```tex
\ucasEmblemBluePath{assets/ucas-emblem-blue.png}
\ucasEmblemWhitePath{assets/ucas-emblem-white.png}
\ucasHorizontalLogoBluePath{assets/ucas-horizontal-logo-blue.png}
\ucasHorizontalLogoWhitePath{assets/ucas-horizontal-logo-white.png}
\ucasBackgroundPath{assets/ucas-background.png}
```

普通内容页使用左上角竖向 logo block。模板没有复杂页脚、学校名居中页脚或横式 logo 顶栏。

## 修改主题颜色

主色集中在 `ucasminimal.sty` 的颜色区：

```tex
\definecolor{UCASBlue}{RGB}{23,73,148}
```

如果你想改成红色，可以直接改这一行，例如：

```tex
\definecolor{UCASBlue}{RGB}{128,30,45}
```

变量名仍叫 `UCASBlue`，是为了减少全局重命名带来的风险。相关的标题、logo block、章节页、代码块背景和行号都会跟随这个主色。深色章节页中较弱的目录项由 `UCASBlueDim` 控制，若主色改成红色，建议也把它改成同色系的浅红灰。

## 开启或关闭弱页码

默认不显示页码。开启右下角浅灰弱页码：

```tex
\ucasShowPageNumbertrue
```

关闭：

```tex
\ucasShowPageNumberfalse
```

## 字体配置：使用思源宋体

推荐安装 **Source Han Serif SC / 思源宋体**。它与当前英文 serif 标题和 small caps 气质更匹配，适合极简学术模板；相比微软雅黑、黑体或偏商业感的无衬线中文字体，思源宋体更接近参考 PDF 的 elegant 风格。

### LaTeX 字体 fallback

模板使用 `fontspec` 和 `xeCJK`，中文主字体按以下顺序自动选择：

1. `Source Han Serif SC`
2. `Noto Serif CJK SC`
3. `SimSun`
4. `FandolSong-Regular`

如果安装了 `Source Han Serif SC SemiBold`，中文加粗和标题优先使用 SemiBold；如果没有 SemiBold，则尝试 `Source Han Serif SC Bold`；如果仍没有，则使用当前 CJK 主字体的加粗 fallback。

英文标题字体保留当前 elegant serif / small caps 风格。数学字体保持 LaTeX 默认或当前稳定配置，代码字体保持 JetBrains Mono 或 Latin Modern Mono，不会改成中文宋体。

### Windows 安装步骤

本工程不包含字体文件，也不会自动下载字体。请手动安装：

1. 从 Adobe Source Han Serif 官方 GitHub Releases 下载 Source Han Serif SC 静态 OTF 字体。
2. 解压下载包。
3. 将 `.otf` 字体文件放入：

```text
tools/fonts/SourceHanSerifSC/
```

4. 在工程根目录运行：

```powershell
powershell -ExecutionPolicy Bypass -File tools/install-source-han-serif-windows.ps1
```

5. 重启 PowerPoint、Word、VS Code 和 TeX 编辑器。

脚本默认做当前用户级安装，不需要管理员权限；如果确实需要系统级安装，可用管理员 PowerShell 运行并添加 `-System` 参数。

### Office 使用方式

在 PowerPoint / Word 的字体框中搜索：

```text
Source Han Serif SC
```

中文标题推荐使用 `Source Han Serif SC SemiBold` 或 `Source Han Serif SC Bold`，中文正文推荐使用 `Source Han Serif SC Regular`。

## VS Code 使用建议

如果使用 LaTeX Workshop，建议 recipe 使用：

```text
latexmk (xelatex)
```

不要只运行单次 `xelatex`，否则目录页可能在第一轮编译后为空。当前工程的 `.vscode/settings.json` 已配置为使用 `latexmk -xelatex`。

## 常见问题

### 为什么必须用 XeLaTeX？

模板使用 `fontspec` 和 `xeCJK` 处理英文和中文字体，必须使用 XeLaTeX。pdfLaTeX 无法可靠处理中文字体。

### 中文字体缺失怎么办？

优先安装 Source Han Serif SC。如果暂时无法安装，模板会自动尝试 `Noto Serif CJK SC`、`SimSun` 和 `FandolSong-Regular`。安装新字体后请重启 Office、编辑器和 TeX 相关程序。

### 图片路径错误怎么办？

确认图片位于 `assets/` 或你设置的图片目录中，并且文件名与 `\includegraphics{...}` 中的路径一致。如果移动了图片，请用对应的 `\ucas...Path{...}` 命令更新路径。

### 目录页为什么空了？

通常是只编译了一轮。运行：

```bash
latexmk -xelatex -g main.tex
```

或连续运行两次 `xelatex main.tex`。

### latexmk 不可用怎么办？

直接运行两次 `xelatex main.tex`。目录和总页数需要至少两次编译才能稳定。

### 为什么没有使用 minted？

`minted` 需要 `shell-escape` 和外部 Python/Pygments 环境。为了让模板更容易独立编译，代码块使用 `listings` 和模板自带的 `ucascodebox`，不需要开启 shell escape。
