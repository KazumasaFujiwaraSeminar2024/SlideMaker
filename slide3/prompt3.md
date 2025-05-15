# LaTeXスライド生成プロンプト（拡張版）

## 基本要件
* LaTeX形式の論文ファイルを読み込み、LuaLaTeXでコンパイル可能なbeamerスライドを作成
* 入力は通常のLaTeXソースコード
* 出力は必ず1枚のスライド（スライドタイトルはなし）

## スライド構成
* 読み込んだ論文ファイルに先行研究がある場合は先行研究の紹介
* 各セクションは箇条書きで記述
    * 可能な限り少ない項目数
* 最後の定理+証明から逆算して作成
    * 作成された文章の長さは自動調整
* 先行研究と定理・証明を確認して生成

## セオレム要約の追加要件
* `\begin{theorem}~\end{theorem}` を自動検出し、**主定理セクションの内容として要点を抽出**
* 複数ある場合、中心的な主張（番号順、または“main”、“重要”などの語を含む）を優先
* 不要な証明や補足は除外する（`\begin{proof},\begin{lemma},\begin{proposition},\begin{corollary},\begin{definition}` 以降は無視）

## preamble
preambleは必ずこれを使用：

```latex
% !TEX program = lualatex
\documentclass[aspectratio=169]{beamer}
\usepackage{luatexja}
\usepackage{amsmath,amssymb}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{fancybox}
\usepackage{tcolorbox}
\usetheme{Copenhagen}
\tcbuselibrary{skins}

% スライドのスタイル設定
\setbeamertemplate{navigation symbols}{}
\setbeamertemplate{footline}[frame number]
\setbeamertemplate{headline}{}

% スライドタイトルのスタイル設定
\setbeamertemplate{frametitle}{%
  \vspace*{-0.2cm}%
  \begin{beamercolorbox}[wd=\paperwidth,ht=1.2cm,dp=0.3cm,leftskip=0pt,rightskip=0pt]{frametitle}
    \hspace*{1em}\Huge\insertframetitle
  \end{beamercolorbox}
}

% セクションのスタイル設定
\newenvironment{sectionblock}[1]{%
  \begin{minipage}{\textwidth}%
    \textbf{\large #1}\par\vspace{0.5em}%
}{%
  \end{minipage}\vspace{1em}%
}

% セクションコンテンツ用マクロ
\newcommand{\sectioncontent}[2]{%
  \begin{sectionblock}{#1}%
    \begin{itemize}%
      #2%
    \end{itemize}%
  \end{sectionblock}%
}
```