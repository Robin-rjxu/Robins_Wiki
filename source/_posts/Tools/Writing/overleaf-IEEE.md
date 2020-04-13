---
title: 使用 Overleaf 编辑 IEEE 论文
toc: true
date: 2020-04-12 21:59:31
tags:
    - writing
    - overleaf
    - Latex
---

## 准备工作

- 为需要编辑的论文建立项目文件夹
- 将相关的资源至项目文件夹
  - 文档内容
    - paper.tex
  - 模板
    - IEEEtran.cls
    - IEEEtran.bst
    - IEEEabrv.bib
  - 参考文献汇总
    - ref.bib
  - 图片
    - 可以建立文件夹 Figs

> **说明**
> LaTex中常见的文件格式有 .tex, .bib, .bst, .cls, .sty, .bbl 等
>
> - 文档内容
>   - .tex 文件也就是我们写文档内容的文件
> - 排版格式相关
>   - .cls 是类文件，通过文档最前面的 \documentclass 命令导入
>   - .sty 是包（风格）文件，通常使用 \usepackage 导入
> - 参考文献相关
>   - .bib 是使用 bibligraphy 方式导入参考文献时，写参考文献的文档
>   - .bbl 是 .bib 其编译之后形成的文件
>   - .bst 设定参考文献出现在文末的方式

## 全局设置 (global )### 类设置

- 声明使用过的类为 IEEEtran，需要存在 IEEEtran.cls 文件
- 设置项 （更多选项见 IEEEtran.cls 内说明）
  - journal: 规定文档类型（可选 conference, journal, technote, peerreview等）
  - twoside: 规定左右页面排版，默认为 oneside，即左右页面使用相同排版，如页脚页眉，页码位置等等

```latex
\documentclass[journal,twoside]{IEEEtran}### 包设置
- 声明使用的样式包
```

- 包说明
  - cite: 优化引用格式
  - graphicx: 插入图片格式
  - amsmath: 数学公式相关格式
  - array: 优化对齐格式
  - subfig: 设置子图及其标题
  - url: 优化链接格式

```latex
% *** CITATION PACKAGES ***
\usepackage{cite}

% *** GRAPHICS RELATED PACKAGES ***
\ifCLASSINFOpdf
\usepackage[pdftex]{graphicx}
\graphicspath{{Figs/}}
\DeclareGraphicsExtensions{.pdf,.jpeg,.jpg,.png}
\else
\fi

% *** MATH PACKAGES ***
\usepackage{amsmath}
\interdisplaylinepenalty=2500

% *** ALIGNMENT PACKAGES ***
\usepackage{array}

% *** SUBFIGURE PACKAGES ***
\ifCLASSOPTIONcompsoc
    \usepackage[caption=false,font=normalsize,labelfont=sf,textfont=sf]{subfig}
\else
    \usepackage[caption=false,font=footnotesize]{subfig}
\fi

% *** PDF, URL AND HYPERLINK PACKAGES ***
\usepackage{url}
```

### 修复断字

```latex
% correct bad hyphenation here
\hyphenation{op-tical net-works semi-conduc-tor}
```

## 页眉页脚 （header and footnote）

声明页眉格式

- 插入位置
  - \markboth: 左右页面均插入页眉
  - \markright: 仅右页面插入页眉
  - \markleft:: 仅左页面插入页眉
- 插入内容
  - 第一个括号 {} 内容在标题页面和偶数页面显示
  - 第二个括号 {} 内容只在除标题页面外，奇数页面显示
  - 页眉默认大写，使用 \MakeLowercase{} 指定使用小写的内容

```latex
\markboth{IEEE Transactions on Circuits and Systems-II: Express Briefs, Vol. XX, No. X, XXX XXXX}%
{Rongjin Xu \MakeLowercase{\textit{et al.}}: A 400 MHz, 8-bit, 1.75-ps Resolution Pipelined-Two-Step Time-to-Digital Converter with Dynamic Time Amplification}
```

### 声明标题

可以使用 `\\` 断行优化显示效果

```latex
\title{A 400 MHz, 8-bit, 1.75-ps Resolution Pipelined-Two-Step Time-to-Digital Converter \\with Dynamic Time Amplification}
```

### 插入标题

```latex
\maketitle
```

## 作者列表、脚注 (author list and footnotes)

### 一般期刊格式

- 使用 `~` 作为空格，声明该词组不被断行
- 使用 `~\IEEEmembership{Student~Member,~IEEE,}` 声明作者 IEEE 会员
- 结尾可以用注释符号 `%` 避免行末空格影响格式

```latex
\author{Rongjin~Xu,~\IEEEmembership{Student~Member,~IEEE,}
        Yuting~Tu,
        Dawei~Ye,~\IEEEmembership{Member,~IEEE,}
        and~C.-J.~Richard~Shi,~\IEEEmembership{Fellow,~IEEE}% <-this % stops a spacee
```

### 作者列表脚注

- 使用 `\thanks{}` 声明作者信息脚注

```latex
\thanks{R. Xu, Y. Tu and D. Ye are with the
State Key Laboratory of ASIC and Systems, Fudan University, Shanghai, China,
201203, and also with the Institute of Brain-Inspired Circuits and Systems (iBiCAS),
Fudan University, Shanghai, China, 201203.}% <-this % stops a space
\thanks{C. –J. R. Shi is with the Department of Electrical and Computer Engineering,
University of Washington, Seattle WA 98195, USA.
}% <-this % stops a space
\thanks{Manuscript received April 19, 2005; revised August 26, 2015.}}}
```

### 一般会议格式

按具体会议格式要求，下面以 RFIC 2020 为例

- 作者列表和信息在 块 `\RFICauthot` 中声明
  - 姓名在 块 `\RFICauthorblockNAME{}` 中声明
  - 标记在 姓名块 中 用 `\RFICauthorrefmark{}` 中声明

```latex
\RFICauthor{%
%
\RFICauthorblockNAME{% Author Names
J. Clerk Maxwell\RFICauthorrefmark{\#1},
Charles Darwin\RFICauthorrefmark{\$},
Michael Faraday\RFICauthorrefmark{*\textasciicircum2},
Andr{\'e} M. Amp{\`e}re\RFICauthorrefmark{\#3}
}% end of \RFICauthorblockNAME
\\%
%
\RFICauthorblockAFFIL{% Author Affiliations
\RFICauthorrefmark{\#}My Lab, My University, USA\\
\RFICauthorrefmark{\$}Gollum World, New Zealand\\
\RFICauthorrefmark{*}My Research, Australia\\
\RFICauthorrefmark{\textasciicircum}SolarSolar, Australia
}% end of \RFICauthorblockAFFIL
\\%
%
\RFICauthorblockEMAIL{% Author Emails
\RFICauthorrefmark{1}J.Clerk.Maxwell@my-uni.edu, \RFICauthorrefmark{2}michael@mr.com.au, \RFICauthorrefmark{3}Ampere@my-uni.edu
}% end of \RFICauthorblockEMAIL
%
}% end of \RFICauthor
```

## 简介与关键词 (abstract and keywords)

### 简介

```latex
\begin{abstract}
    text
\end{abstract}
```

### 关键词

[参考关键词](https://www.ieee.org/content/dam/ieee-org/ieee/web/org/pubs/taxonomy_v101.pdf)

```latex
\begin{IEEEkeywords}
    keywords
\end{IEEEkeywords}
```

## 正文与分节 (text and section)

文档内容全部包含在 块 `document` 中，包括

- 标题
- 作者列表与脚注
- 正文
- 致谢
- 附录
- 参考文献
- 作者简历

```latex
\begin{document}

all contents

\end{document}}
```

期刊论文开头需要使用特殊格式 dropletter，即第一个单词大写，第一个字母占两行。会议论文中通常没有该要求。

```latex
\IEEEPARstart{T}{ime}
```

有标题序号的分节使用 `\section{title}\label{label_name}{}` ，如正文的分节;

无标题序号的分节使用 `\section*{title}\label{label_name}{}` ，如致谢、附录等。

如无需引用，标签可省略。

```latex
\section{The Proposed Pipelined-Two-Step TDC Architecture}
```

更低级别分节 使用 `\subsection{title}\label{label_name}{}`。

```latex
\subsection{Overall architecture}
```

## 图片 (figure)

图片的插入使用 graphicx 包实现

- 块命令为 `\begin{figure}`
  - 单栏图片 `\begin{figure}`
  - 跨栏图片 `\begin{figure*}`
- 插入单个图片
  - 命令为 `\includegraphics[options]{file_name}`
  - 插入标题命令为 `\caption{text}`
  - 插入标签命令为 `\label{}`
- 插入子图
  - 命令为 `\subfloat[sub_caption]{\includegraphics[options]{file_name}`
- 子图标题
  - 通常子图只编号，如 a，b 等
  - 则子图标题留空即可，如 `[]`
  - 子图之间可以使用 `\hfil` 增加间隔空间
  - 插入组图标题和标签等，同上

### 单栏图片

```latex
% floating figure using the graphicx package
\begin{figure}[!t]
\centering
\includegraphics[width=2.5in]{cat1}
\caption{Simulation results for the network.}
\label{fig_sim1}
\end{figure}}
```

### 跨栏图片

```latex
\begin{figure*}[!t]
\centering
\subfloat[Case I]{\includegraphics[width=2.5in]{cat1}%
\label{fig_first_case}}
\hfil
\subfloat[Case II]{\includegraphics[width=2.5in]{cat1}%
\label{fig_second_case}}
\caption{Simulation results for the network.}
\label{fig_sim2}
\end{figure*}}
```

## 表格 (table)

表格块 table

- 插入
  - 插入单栏表格 `\begin{table}`
  - 插入跨栏表格`\begin{table*}`
- 表格标题 `\caption{title`} 
- 引用标签 `\label{table_label}`
- 格式设置
  - 增加行空间  `\renewcommand{\arraystretch}{1.3}`
  - 表格整体居中 `\centering`
- 表格内容 `\begin{tabular}{|c||c|}`
  - 单元格对齐设置 `{|l|c|r|}`
  - 通过添加 | 来表示是否需要绘制竖线
  - 行间横线 `\hline`
  - 内容分隔符 `&`
  - 换行 `\\`
- 其他复杂表格格式可以通过相关命令实现

### 单栏表格

```latex
\begin{table}[!t]
\renewcommand{\arraystretch}{1.3}
\caption{An Example of a Table}
\label{table_example}
\centering
\begin{tabular}{|c||c|}
\hline
One & Two\\
\hline
Three & Four\\
\hline
\end{tabular}
\end{table}e
```

```latex
\setlength{\tabcolsep}{1.25mm}%
\renewcommand{\arraystretch}{1.2}% for the vertical padding of table cells
\newcommand{\CPcolumnonewidth}{not used}%
\newcommand{\CPcolumntwowidth}{23mm}%
\newcommand{\CPcolumnthreewidth}{12mm}%
\newcommand{\CPcolumnfourwidth}{33mm}%
\begin{table}
\caption{Font sizes for papers.  Table caption with more than one line must be justified.  Table caption with just one line must be centered.}
\small% RFIC: need this to get the 9pt text size in table cells % TODO is correct ?
\centering
\begin{tabular}{|l|l|l|l|}\hline
\multirow{2}{6mm}{\parbox{8mm}{{\bfseries Font Size}}} & \multicolumn{3}{c|}{\raisebox{-0.25mm}{\bfseries Appearance (in Times New Roman or Times)}}\\ \cline{2-4}
& \raisebox{-0.25mm}{regular} & \raisebox{-0.25mm}{\bfseries bold} & \raisebox{-0.25mm}{\itshape italic} \\ \hline
8 & \parbox[t]{\CPcolumntwowidth}{\strut table caption,\\figure caption,\\figure label,\\reference item\strut} & & reference item (partial)\\ \hline
9 & cell in a table & \parbox[t]{\CPcolumnthreewidth}{abstract, keywords} & \parbox[t]{\CPcolumnfourwidth}{also in bold:\\abstract section heading,\\keywords section heading\strut} \\ \hline
10 & \parbox[t]{\CPcolumntwowidth}{level-1 heading,\\paragraph,\\equation\strut} & & \parbox[t]{\CPcolumnfourwidth}{level-2 heading,\\level-3 heading} \\ \hline
12 & \parbox[t]{\CPcolumntwowidth}{{author name},\\author affiliation,\\email address\strut} & & \\ \hline
18 & title & & \\ \hline
\end{tabular}
\label{tab:fontsizes}
\end{table}
}% end of group enclosing the table
```

### 跨栏表格

```latex
\setlength{\tabcolsep}{1mm}%
\newcommand{\CPcolumnonewidth}{50mm}%
\newcommand{\CPcolumntwowidth}{91mm}%
\newcommand{\CPcell}[1]{\hspace{0mm}\rule[-0.3em]{0mm}{1.3em}#1}%
\newcommand{\CPcellbox}[1]{\parbox{90mm}{\hspace{0mm}\rule[-0.3em]{0mm}{1.3em}#1\strut}}%
\begin{table*}
\caption{Main predefined styles in WORD.  Table which spans 2 columns must be placed either at top of a page or at bottom of a page.}
\small% need this to get the 9pt text size in table cells
\centering
\begin{tabular}{|l|l|}\hline
\parbox{\CPcolumnonewidth}{\CPcell{\bfseries Style Name}} & \parbox{\CPcolumntwowidth}{\CPcell{\bfseries To Format \ldots}} \\ \hline
\CPcell{RFIC Title} & \CPcell{title} \\ \hline
\CPcell{RFIC Author Block} & \CPcell{author name, author affiliation, author email address} \\ \hline
\CPcell{RFIC Author Block + Superscript} & \CPcellbox{affiliation marker or email address marker after an author name,\\ marker before an affiliation or an email address} \\ \hline
\CPcell{RFIC Abstract/Keywords} & \CPcell{abstract, keywords} \\ \hline
\CPcell{RFIC Abstract/Keywords + Italic} & \CPcell{abstract section heading, keywords section heading} \\ \hline
\CPcell{RFIC Heading 1} & \CPcell{1st level section heading} \\ \hline
\CPcell{RFIC Heading 2} & \CPcell{2nd level section heading} \\ \hline
\CPcell{RFIC Heading 3} & \CPcell{3rd level section heading} \\ \hline
\CPcell{RFIC Paragraph} & \CPcell{paragraph} \\ \hline
\CPcell{RFIC Figure} & \CPcell{figure to be centered} \\ \hline
\CPcell{RFIC Figure Label} & \CPcell{label placed below a component of a figure, e.g. (a) in Fig.2} \\ \hline
\CPcell{RFIC Caption Single-Line} & \CPcell{figure or table caption containing one line} \\ \hline
\CPcell{RFIC Caption Multi-Lines} & \CPcell{figure or table caption containing more than one line} \\ \hline
\CPcell{RFIC Ack/Ref Heading} & \CPcell{acknowledgment section heading, references section heading} \\ \hline
\CPcell{RFIC Reference Item} & \CPcell{reference item} \\ \hline
\end{tabular}
\label{tab:wordstyles}
\end{table*}
}
```

## 公式 (equation)

```latex
\begin{equation}
A+B=C \label{lable_name}
\end{equation}
```

## 标签与引用 (label and citation)

分节、公式图片、表格等内容可以使用 

标签声明`\label{label_name}`，引用则使用 `\cite{label_name}`

## 致谢与附录 (acknowledgement and appendix )

### 致谢

致谢采用无编号分节格式 `\section*{title}`。

```latex
\section*{Acknowledgment}
This work is supported by the Science and Technology Commission of Shanghai Municipality under Grant 2018SHZDZX01.
```

### 附录

附录采用命令块 `\appendices`

内部可以分节 `\section{title}`

```latex
\appendices

\section{Proof of the First Zonklar Equation}
Appendix one text goes here.

\section{}
Appendix two text goes here..
```

## 参考文献 (reference)

### 使用 bibtex 管理参考文献使用 bibtex

- 依赖文件
  - 全局格式
    - IEEEtran.bts
    - IEEEabrv.bib
  - 参考文献列表
    - refbib.bib
    - 官方示例 IEEEexample.bib
- 各学术网站与管理软件都提供导出 bibtex 功能
- 格式控制
  - 自动根据引用顺序给文献编号
  - 出版物名称控制
    - 官方参考名称管理
      - 缩写使用 IEEEabrv
      - 全称使用 IEEEfull
      - 不提供会议名称缩写
    - 可以根据格式要求自定义管理
      - 在参考文献列表中使用 标记符号即可
        - 标记符号使用 ，如 `journal=IEEE_J_CASII`

```latex
\bibliographystyle{IEEEtran}
\bibliography{IEEEabrv, refbib}
```

参考文献列表实例

```latex
@IEEEtranBSTCTL{IEEEexample:BSTcontrol,
  CTLuse_article_number     = "yes",
  CTLuse_paper              = "yes",
  CTLuse_url                = "yes",
  CTLuse_forced_etal        = "no",
  CTLmax_names_forced_etal  = "10",
  CTLnames_show_etal        = "1",
  CTLuse_alt_spacing        = "yes",
  CTLalt_stretch_factor     = "4",
  CTLdash_repeated_names    = "yes",
  CTLname_format_string     = "{f.~}{vv~}{ll}{, jj}",
  CTLname_latex_cmd         = "",
  CTLname_url_prefix        = "[Online]. Available:"
}

@ARTICLE{ref_yizhe,  
author={Y. {Hu} and T. {Siriburanon} and R. B. {Staszewski}},
journal=IEEE_J_CAS,
title={Intuitive Understanding of Flicker Noise Reduction via Narrowing of Conduction Angle in Voltage-Biased Oscillators},
year={2019},  
volume={66},  
number={12},  
pages={1962-1966},
}
```

### 简单罗列参考文献格式控制

- 根据 `bibitem{}` 顺序编号
- 使用 `\emph{text}` 规定出版物名称的斜体
- 使用 一些空间控制命令，具体参看类声明文件 IEEEtran.cls

```latex
\begin{thebibliography}{1}
\bibitem{IEEEhowto:kopka}
H.~Kopka and P.~W. Daly, \emph{A Guide to \LaTeX}, 3rd~ed.\hskip 1em plus
  0.5em minus 0.4em\relax Harlow, England: Addison-Wesley, 1999.
\end{thebibliography}}
```

## 作者简历 (biography)

插入作者简介

- 包含照片的作者简介
  - 命令块 `\begin{IEEEbiography}`
  - 选项 []
  - 使用 `\includegraphics` 插入照片
- 无照片的作者简介
  - 命令块 `\begin{IEEEbiographynophoto}`
- 格式控制
  - 使用 `\vfill` 调整简介块的位置

### 包含照片的作者简介

```latex
\begin{IEEEbiography}[{\includegraphics[width=1in,height=1.25in,clip,keepaspectratio]{photo_file}}]{author_name}
Biography text here.
\end{IEEEbiography}
```

### 不含照片的作者简介

```latex
\begin{IEEEbiographynophoto}{author_name}
Biography text here.
\end{IEEEbiographynophoto}
```

## 其他控制命令

- `~`  作为空格，声明该词组不被断行
- 增加空间
  - `\hfill`
  - `\vfill`
- 另起一页，平衡位置
  - `\newpage`

## References

> - [IEEE author center](http://ieeeauthorcenter.ieee.org)
> - [The IEEEtran Homepage](http://www.michaelshell.org/tex/ieeetran/) \
> - [CTAN Package IEEEtran](https://ctan.org/pkg/ieeetran) \
> - [IEEE editorial style manual for authors](http://ieeeauthorcenter.ieee.org/wp-content/uploads/IEEE_Style_Manual.pdf) \
> - [IEEE reference guide](https://ieeeauthorcenter.ieee.org/wp-content/uploads/IEEE-Reference-Guide.pdf) \
