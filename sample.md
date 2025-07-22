# 統合スライド生成プロンプトテンプレート

## 目的
LaTeX形式の論文ソースを全文解析してから、LuaLaTeXでコンパイル可能なbeamerスライドを自動生成する。
1スライドまたは複数スライドを作成し、内容に応じて自動的に構成・要約を行う。

## 入力
- 通常のLaTeXソース(.tex)形式の論文ファイル
- セクションマーカーやtheorem環境が含まれる

## 出力仕様
- 出力は1~3枚のスライド
- 各スライドは以下のいずれかのカテゴリに対応
  - 概要(背景・目的・主張をまとめた要約)
  - 主定理(もっとも重要な定理の要約)
  - 定義(主要な定義の抽出)
  - 補助命題・補題(主定理を支える要素)
  - 証明・評価(数式による主張の論理展開)

## スライド構成ルール
### 共通ルール
  - 各スライドは
  ```tex
  \frametitle{...}
  ```
  によって明示されたタイトルを持つ

  - セクション内はすべて
  ```tex
  \sectioncontent{タイトル}{箇条書き項目}
  ```
  形式で記述

  - 箇条書きは可能な限り少なく、簡潔な表現(最大3項目)
  - 必要に応じて途中式なども記述(証明・主定理など)

### スライド種別ごとの要約ルール
  - スライド種別=概要、タイトル=概要、内容=背景・目的・主張の短い要約、要約文字数目安=(背景：30~40文字、目的：20~30文字、主張：40~50文字)

  - スライド種別=主定理、タイトル=主定理、内容=最重要な定理とその要点、要約文字数目安=(40~50文字(箇条書き)+必要な数式)

  - スライド種別=定義、タイトル=定義、内容=論文内の中心的な定義、要約文字数目安=(定義ごとに40~50文字)

  - スライド種別=補助命題、タイトル=補助、内容=主定理を支える命題・補助・定義、要約文字数目安=(数式付き箇条書き(必要に応じて改行))

  - スライド種別=証明、タイトル=証明、内容=不等式や評価を含む数式展開、要約文字数目安=(数式・途中式(箇条書きまたは改行付き))

## スライド構成概要
% --- Slide 1: 概要 ---
% --- Slide 2: 主定理 ---
% --- Slide 3: 証明 ---
% --- Slide 4: 定義 ---
% --- Slide 5: 補助命題 ---

## 内容抽出方針
### 定理・命題・定義の抽出
  - 以下の環境を自動検出して内容抽出
  ```tex
  \begin{theorem}...\end{theorem}
  \begin{definition}...\end{definition}
  \begin{proposition}...\end{proposition}
  \begin{lemma}...\end{lemma}
  ```

  - 検出されたものの中で、次の基準で「主要なもの」を優先的に使用
    - 出現順(番号順)
    - 環境タイトルに"main","重要","central"を含む
  
  - proof,corollaryなど補足要素は無視
  - 「定義風」の記述(例：「～定義する」)が検出された場合、補足的に追加可能

## スライドタイトル自動決定の原則
- 優先順位=高、マーカーor検出対象=(%% MARK begin定義～%% MARK end定義またはdefinition環境)、タイトル=定義
- 優先順位=中、マーカーor検出対象=(theorem環境(主定理))、タイトル=主定理
- 優先順位=低、マーカーor検出対象=(%% MARK begin背景、%% MARK begin目的、%% MARK begin主張など)、タイトル=概要
- 優先順位=任意、マーカーor検出対象=(proposition/lema環境)、タイトル=補助命題

## LaTeX preamble(共通)
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

\setbeamertemplate{navigation symbols}{}
\setbeamertemplate{footline}[frame number]
\setbeamertemplate{headline}{}

\setbeamertemplate{frametitle}{%
  \vspace*{-0.2cm}%
  \begin{beamercolorbox}[wd=\paperwidth,ht=1.2cm,dp=0.3cm,leftskip=0pt,rightskip=0pt]{frametitle}
    \hspace*{1em}\Huge\insertframetitle
  \end{beamercolorbox}
}

\newenvironment{sectionblock}[1]{%
  \begin{minipage}{\textwidth}%
    \textbf{\large #1}\par\vspace{0.5em}%
}{%
  \end{minipage}\vspace{1em}%
}

\newcommand{\sectioncontent}[2]{%
  \begin{sectionblock}{#1}%
    \begin{itemize}%
      #2%
    \end{itemize}%
  \end{sectionblock}%
}
```
## オプション(任意)
- \textbfの使用制限などは全スライドに一貫したルールで明記
