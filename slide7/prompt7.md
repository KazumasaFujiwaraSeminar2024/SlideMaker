# LaTeXスライド生成プロンプト（拡張版）

## 基本要件
* LaTeX形式の論文ファイルを読み込み、LuaLaTeXでコンパイル可能なbeamerスライドを作成
* 入力は通常のLaTeXソースコード
* 読み込んだファイルの要点で抜けている部分や書式が一般的でないものがある場合はそれを以下にしたがって抽出して修正してください
  * 抜けている要点は最新のブラウザの情報から分析して補い記述する
  * 一般的でない書式は以下に記載されているpreambleで代用する
* スライドの枚数は1枚

## スライド構成
* 主定理を示すのに必要な命題や定義を記述
  * 「主定理」とは論文の一番重要な定理を記述
  * 主定理は記述しない
  * 必要な数式を途中式と共に全て記述
  * 箇条書きで記述
  * 可能な限り少ない項目数
  * 文字の大きさは自動調整
  * 作成された文章の長さは自動調整
  * \textbf{}は出力しない

## セオレム要約の追加要件
* `\begin{theorem}~\end{theorem}`,`\begin{definition}~\end{definition}`を自動検出し、**解析セクションの内容として要点を抽出**
  * 複数ある場合、中心的な命題や定義（番号順、または“main”、“重要”などの語を含む）を優先
  * 検出されない場合は他の関連環境（例：定義風に書かれた内容）や明示的に「定義」や「定義する」と記載されている部分の抽出
* 複数ある場合、中心的な命題や定義（番号順、または“main”、“重要”などの語を含む）を優先

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
\setbeamertemplate{frametitle}{}

% セクションコンテンツ用マクロ
\newcommand{\sectioncontent}[2]{%
  \begin{sectionblock}{#1}%
    \begin{itemize}%
      #2%
    \end{itemize}%
  \end{sectionblock}%
}
```