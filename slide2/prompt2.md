# LaTeXスライド生成プロンプト

## 基本要件
* LaTeX形式の論文ファイルを読み込み、LuaLaTeXでコンパイル可能なbeamerスライドを作成
* 入力は通常のLaTeXソースコード
* 出力は必ず1枚のスライド（スライドタイトル：「概要）
* スライド内の文字の大きさは5ptに統一

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

% 見出しのスタイル設定
\setbeamertemplate{tcolorbox}

% 見出しの装飾設定
\newcommand{\decoratedsection}[2]{%
  \begin{tcolorbox}[
                    colback=green!10!white,
                    colbacktitle=green!40!white,
                    coltitle=black,
                    enhanced,
                    attach boxed title to top left={xshift=1cm,yshift=-2mm},
                    boxed title style={colback=green!40!white,colframe=green!40!white}]
    \color{black}\Large\bfseries #1
    \vspace{0.5cm}
    \normalsize\normalfont #2
  \end{tcolorbox}
}
```