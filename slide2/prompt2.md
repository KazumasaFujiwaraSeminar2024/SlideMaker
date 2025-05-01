# LaTeXスライド生成プロンプト（拡張版）

## 基本要件
* LaTeX形式の論文ファイルを読み込み、LuaLaTeXでコンパイル可能なbeamerスライドを作成
* 入力は通常のLaTeXソースコード
* 出力は必ず1枚のスライド（スライドタイトル：「概要」）

## スライド構成
* 4つのセクション：「背景」「目的」「主定理」「方法」
* 各セクションは箇条書き形式で記述
  * 可能な限り少ない項目数
* 論文の主張や結論を中心に要約
  * 各セクションの要約の長さは自動調整  
    * 背景：30-40文字  
    * 目的：20-30文字  
    * 主定理：40-50文字  
    * 方法：30-40文字
* 目的と主定理の接続を確認して生成

## セオレム要約の追加要件
* `\begin{theorem}` などのセオレム環境を自動検出し、**主定理セクションの内容として要点を抽出**
* 抽出対象の環境：`theorem`, `lemma`, `proposition`, `definition`, `corollary`
* セオレムの文中で重要語（定義、条件、結論）を優先して取り出す
* 複数ある場合、中心的な主張（番号順、または“main”、“重要”などの語を含む）を優先
* 不要な証明や補足は除外する（`\begin{proof}` 以降は無視）

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