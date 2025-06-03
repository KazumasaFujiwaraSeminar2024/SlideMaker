# LaTeXスライド生成プロンプト（拡張版）

## 基本要件
* LaTeX形式の論文ファイルを読み込み、LuaLaTeXでコンパイル可能なbeamerスライドを作成
* 入力は通常のLaTeXソースコード
* 読み込んだファイルの要点で抜けている部分や書式が一般的でないものがある場合はそれを以下にしたがって抽出して修正してください
  * 抜けている要点は最新のブラウザの情報から分析して補い記述する
  * 一般的でない書式は以下に記載されているpreambleで代用する
* スライドタイトル：「概略」「主定理」「評価」
    * スライドの枚数は3枚以内


## スライド構成
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
* 「評価」では数学的に評価した数式を全て記述
  * 評価するとは「説明したいものを不等式の記号を使って、大小関係などを表す」ことをいう
  * 必要な数式を途中式と共に全て記述
  * 箇条書きで記述
    * 計算が途中で改行される場合は箇条書きの項目は出力しない
  * 補足する補題も記述
  * 可能な限り少ない項目数
  * 作成された文章の長さは自動調整
  * \textbf{}は出力しない



## セオレム要約の追加要件
* `\begin{theorem}~\end{theorem}` を自動検出し、**結論セクションの内容として要点を抽出**
  * 複数ある場合、中心的な主張（番号順、または“main”、“重要”などの語を含む）を優先
  * 検出されない場合は他の関連環境（例：定義風に書かれた内容）や明示的に「定義」や「定義する」と記載されている部分の抽出
* 複数ある場合、中心的な主張（番号順、または“main”、“重要”などの語を含む）を優先

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