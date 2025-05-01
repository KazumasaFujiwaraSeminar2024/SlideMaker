# LaTeXスライド生成プロンプト

## 基本要件
* LaTeX形式の論文ファイルを読み込み、LuaLaTeXでコンパイル可能なbeamerスライドを作成
* 入力は通常のLaTeXソースコード
* 出力は必ず1枚のスライド（スライドタイトル：「概要）

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

## preamble
preambleは必ずこれを使用

```latex
% !TEX program = lualatex
\documentclass[aspectratio=169]{beamer}
\usepackage{luatexja}
\usepackage{amsmath,amssymb}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{fancybox}
\usepackage{tcolorbox}
\tcbuselibrary{skins}

% スライドのスタイル設定
\setbeamertemplate{navigation symbols}{}
\setbeamertemplate{footline}[frame number]
\setbeamertemplate{headline}{}

%スライドタイトルのスタイル設定
\newcommand{\frametitle}{概要}

% セクションのスタイル設定
\newcommand{\sectioncontent}[2]{%
  \begin{minipage}[t]{\textwidth}
    \textbf{\large #1}\\[0.5em]
    #2
  \end{minipage}
  \vspace{1em}
}
```