
## 統合スライド生成プロンプトテンプレート

## 目的

LaTeX形式の論文ソースを全文解析してから、LuaLaTeXでコンパイル可能なbeamerスライドを自動生成する。1スライドまたは複数スライドを作成し、内容に応じて自動的に構成・要約を行う。

---

## 入力

通常のLaTeXソース(.tex)形式の論文ファイル
セクションマーカーやtheorem環境が含まれる

---

## 出力仕様

出力は1\~3枚のスライド
各スライドは以下のいずれかのカテゴリに対応

* **概要**（背景・目的・主張をまとめた要約）
* **主定理**（もっとも重要な定理の要約）
* **定義**（主要な定義の抽出）
* **補助命題・補題**（主定理を支える要素）
* **証明・評価**（数式による主張の論理展開）

---

## 定義風の記述の検出（構文確認の代行）

ファイル全文を読み取り、**theoremや定義に似た自然言語表現**（例：「〜を定義する」「〜であることを示す」など）から、**スライド生成に適した候補を手動で抽出・分類**する

# スライド構成ルール

### 共通ルール

各スライドは `\frametitle{...}` によって明示されたタイトルを持つ
セクション内はすべて `\sectioncontent{タイトル}{箇条書き項目}` 形式で記述

* 箇条書きは可能な限り少なく、簡潔な表現（最大3項目）
* 必要に応じて途中式なども記述（証明・主定理など）

### スライド種別ごとの要約ルール

| スライド種別 | タイトル | 内容            | 要約文字数目安                             |
| ------ | ---- | ------------- | ----------------------------------- |
| 概要     | 概要   | 背景・目的・主張の短い要約 | 背景: 30〜40文字、目的: 20〜30文字、主張: 40〜50文字 |
| 主定理    | 主定理  | 最重要な定理とその要点   | 箇条書き40〜50文字＋必要な数式                   |
| 定義     | 定義   | 論文内の中心的な定義    | 各定義ごとに40〜50文字                       |
| 補助命題   | 補助   | 主定理を支える命題・補助  | 数式付き箇条書き（必要に応じて改行）                  |
| 証明     | 証明   | 数式・不等式を含む論理展開 | 数式・途中式（箇条書きまたは改行付き）                 |

---

### スライド構成概要（例）

```
% --- Slide 1: 概要 ---  
% --- Slide 2: 主定理 ---  
% --- Slide 3: 証明 ---  
% --- Slide 4: 定義 ---  
% --- Slide 5: 補助命題 ---
```

---

## 内容抽出方針

### 定理・命題・定義の抽出

以下の環境を自動検出して内容抽出：

* `\begin{theorem}...\end{theorem}`
* `\begin{definition}...\end{definition}`
* `\begin{proposition}...\end{proposition}`
* `\begin{lemma}...\end{lemma}`

**優先度**：

* 出現順（番号順）
* 環境タイトルに `"main"`, `"重要"`, `"central"` を含むもの優先
* `proof`, `corollary` など補足環境は原則無視

---

### スライドタイトル自動決定の原則

* 優先=高：`%% MARK begin定義～%% MARK end定義` または `definition環境` → タイトル: 定義
* 優先=中：`theorem環境`（主定理）→ タイトル: 主定理
* 優先=低：`%% MARK begin背景/目的/主張` → タイトル: 概要
* 任意：`proposition/lemma環境` → タイトル: 補助命題

---

## LaTeX preamble（共通）

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

---

## オプション（任意）

* `\textbf` の使用制限などは、全スライドに一貫したルールで指定可能

---

必要であれば、マークアップ構文や自然言語の文構造を事前解析して、より適切な構成分類を行う支援も行います。
