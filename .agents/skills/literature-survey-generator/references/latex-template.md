# LaTeX Literature Survey Template

Use this as the starting point for `survey.tex`. Adapt the title, section structure, terminology, and table columns to the discipline and evidence base.

```latex
\documentclass[12pt]{article}

\usepackage{amsmath,amssymb}
\usepackage[margin=1in]{geometry}
\usepackage[colorlinks=true,citecolor=blue,linkcolor=blue,urlcolor=blue]{hyperref}
\usepackage{booktabs}
\usepackage{natbib}
\usepackage{setspace}
\usepackage{array}
\usepackage{longtable}

\bibliographystyle{plainnat}

\title{[TOPIC]: A Literature Survey}
\author{[AUTHOR]}
\date{[MONTH YEAR]}

\begin{document}
\maketitle

\begin{abstract}
% State the question, scope, search period, evidence base, major themes,
% principal conclusions, and important limitations.
\end{abstract}

\doublespacing

\section{Introduction}
% Rationale, scope, definitions, prior reviews, and contribution.

\section{Methods}
% Databases/sources, search dates, queries, eligibility criteria,
% screening, extraction, and synthesis approach.

\section{Summary of Included Studies}

\begin{longtable}{>{\raggedright\arraybackslash}p{2.8cm} >{\raggedright\arraybackslash}p{2.2cm} >{\raggedright\arraybackslash}p{3.3cm} >{\raggedright\arraybackslash}p{2.3cm} >{\raggedright\arraybackslash}p{4cm}}
\caption{Summary of included studies}\label{tab:summary}\\
\toprule
Authors & Year/Source & Design or Data & Population/Scope & Main Contribution \\
\midrule
\endfirsthead
\toprule
Authors & Year/Source & Design or Data & Population/Scope & Main Contribution \\
\midrule
\endhead
% One row per included study.
\bottomrule
\end{longtable}

\section{[Thematic Synthesis 1]}
% Compare studies, methods, findings, limitations, and disagreements.

\section{[Thematic Synthesis 2]}

\section{Methodological Comparison}

\section{Limitations and Evidence Gaps}

\section{Future Research Directions}

\section{Conclusion}

\bibliography{references}
\end{document}
```

## Citation conventions

- `\citet{key}` for narrative citations.
- `\citep{key}` for parenthetical citations.
- `\citep{key1,key2}` for multiple citations.
- Citation keys follow `{firstauthorlastname}{year}{keyword}`.
