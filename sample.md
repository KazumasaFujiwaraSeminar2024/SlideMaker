# slide全体プロンプト

## slide2

### 基本要件
* LaTeX形式の論文ファイルを読み込み、LuaLaTeXでコンパイル可能なbeamerスライドを作成
* 入力は通常のLaTeXソースコード
* 出力は必ず1枚のスライド（スライドタイトル：「概要」）

### スライド構成
* 3つのセクション：「背景」「目的」「主定理」
* 各セクションは箇条書き形式で記述
  * 可能な限り少ない項目数
* 論文の主張や結論を中心に要約
  * 各セクションの要約の長さは自動調整  
    * 背景：30-40文字  
    * 目的：20-30文字  
    * 主定理：40-50文字  
* 目的と主定理の接続を確認して生成

### セオレム要約の追加要件
* `\begin{theorem}~\end{theorem}` を自動検出し、**主定理セクションの内容として要点を抽出**
* 複数ある場合、中心的な主張（番号順、または“main”、“重要”などの語を含む）を優先
* 不要な証明や補足は除外する（`\begin{proof},\begin{lemma},\begin{proposition},\begin{corollary},\begin{definition}` 以降は無視）

### preamble
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

## slide3

### 基本要件
* LaTeX形式の論文ファイルを読み込み、LuaLaTeXでコンパイル可能なbeamerスライドを作成
* 入力は通常のLaTeXソースコード
* 出力は必ず1枚のスライド（スライドタイトル：「主定理」）

### スライド構成
箇条書き形式で記述
  * 可能な限り少ない項目数
* 論文の主張や結論を中心に要約
  * 要約の長さは自動調整  
    * 主定理：40-50文字  
* 目的と主定理の接続を確認して生成

### セオレム要約の追加要件
* `\begin{theorem}~\end{theorem}` を自動検出し、**主定理セクションの内容として要点を抽出**
* 複数ある場合、中心的な主張（番号順、または“main”、“重要”などの語を含む）を優先
* 不要な証明や補足は除外する（`\begin{proof},\begin{lemma},\begin{proposition},\begin{corollary},\begin{definition}` 以降は無視）

### preamble
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

## slide4

### 基本要件
* LaTeX形式の論文ファイルを読み込み、LuaLaTeXでコンパイル可能なbeamerスライドを作成
* 入力は通常のLaTeXソースコード
* 出力は必ず1枚のスライド（スライドタイトル：「定義」）

### スライド構成
* 論文で使われている定義を中心に記述
* 各セクションは箇条書きで記述
    * 可能な限り少ない項目数
    * 作成された文章の長さは自動調整
      * 定義：40-50文字

### セオレム要約の追加要件
* `\begin{Definition}`は`\begin{definition}`と同じpreambleである
* `\begin{definition}~\end{definition}` を自動検出し、**定義セクションの内容として要点を全て抽出**
  * 検出されない場合は他の関連環境（例：定義風に書かれた内容）や明示的に「定義」や「定義する」と記載されている部分の抽出
* 複数ある場合、中心的な主張（番号順、または“main”、“重要”などの語を含む）を優先
* 不要な証明や補足は除外する（`\begin{proof},\begin{lemma},\begin{proposition},\begin{corollary},\begin{theorem}` 以降は無視）

### preamble
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

## slide7

### 基本要件
* LaTeX形式の論文ファイルを読み込み、LuaLaTeXでコンパイル可能なbeamerスライドを作成
* 入力は通常のLaTeXソースコード
* 読み込む際は、以下のマーカーを使用して読み込む
* 読み込んだファイルの要点で抜けている部分や書式が一般的でないものがある場合はそれを以下にしたがって抽出して修正してください
  * 抜けている要点は最新のブラウザの情報から分析して補い記述する
  * 一般的でない書式は以下に記載されているpreambleで代用する
* スライドに記述する内容の優先順位は「定義」>「命題」>「補題」
    * 「定義」「命題」「補題」以外は無視
* スライドタイトルは以下のpreambleを使用する
    * スライドの枚数は1枚

### スライド構成
* 主定理を示すのに必要な命題や定義を記述
  * 「主定理」とは論文の一番重要な定理のこと
  * 主定理は記述しない
  * 必要な数式を途中式と共に全て記述
  * 箇条書きで記述
  * 可能な限り少ない項目数
  * 文字の大きさは自動調整
  * 作成された文章の長さは自動調整
  * \textbf{}は出力しない

### セオレム要約の追加要件
* `\begin{theorem}~\end{theorem}`,`\begin{definition}~\end{definition}`を自動検出し、**解析セクションの内容として要点を抽出**
  * 複数ある場合、中心的な命題や定義（番号順、または“main”、“重要”などの語を含む）を優先
  * 検出されない場合は他の関連環境（例：定義風に書かれた内容）や明示的に「定義」や「定義する」と記載されている部分の抽出
* 複数ある場合、中心的な命題や定義（番号順、または“main”、“重要”などの語を含む）を優先

### preamble
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
\setbeamertemplate{frametitle}{}

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

### マーカー
マーカーは以下のとおり

```tex
%begin_背景
%end_背景
%begin_目的
%end_目的
%begin_主定理
%end_主定理
%begin_主張
%end_主張
%begin_結論
%end_結論
%begin_定義
%end_定義
%begin_命題
%end_命題
%begin_補題
%end_補題
%begin_証明
%end_証明
```

## slide8

### 基本要件
* LaTeX形式の論文ファイルを読み込み、LuaLaTeXでコンパイル可能なbeamerスライドを作成
* 入力は通常のLaTeXソースコード
* 読み込んだファイルの要点で抜けている部分や書式が一般的でないものがある場合はそれを以下にしたがって抽出して修正してください
  * 抜けている要点は最新のブラウザの情報から分析して補い記述する
  * 一般的でない書式は以下に記載されているpreambleで代用する
* スライドタイトル：「概略」「主定理」「証明」
    * スライドの枚数は3枚以内

### スライド構成
* 「概略」では論文の結論と主張を要約したものを記述
  * 結論と主張は出力しない
  * 箇条書きで記述
  * 可能な限り少ない項目数
  * 作成された文章の長さは自動調整
* 「主定理」では論文の一番重要な定理を記述
  * 必要な数式を途中式と共に全て記述
  * 箇条書きで記述
  * 可能な限り少ない項目数
  * 作成された文章の長さは自動調整
  * \textbf{}は出力しない
* 「証明」では数学的に評価した数式を全て記述
  * 補足する補題も記述
  * 評価するとは「説明したいものを不等式の記号を使って、大小関係などを表す」ことをいう
  * 必要な数式を途中式と共に全て記述
  * 計算が途中で改行される場合は箇条書きではなく改行して記述
    * 改行する際は「\\」を使用
  * 可能な限り少ない項目数
  * 作成された文章の長さは自動調整
  * \textbf{}は出力しない



### セオレム要約の追加要件
* `\begin{theorem}~\end{theorem}` を自動検出し、**結論セクションの内容として要点を抽出**
  * 複数ある場合、中心的な主張（番号順、または“main”、“重要”などの語を含む）を優先
  * 検出されない場合は他の関連環境（例：定義風に書かれた内容）や明示的に「定義」や「定義する」と記載されている部分の抽出
* 複数ある場合、中心的な主張（番号順、または“main”、“重要”などの語を含む）を優先

### preamble
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

% セクションコンテンツ用マクロ
\newcommand{\sectioncontent}[2]{%
  \begin{sectionblock}{#1}%
    \begin{itemize}%
      #2%
    \end{itemize}%
  \end{sectionblock}%
}
```