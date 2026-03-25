# Empirical Economics Paper Template — Applied Micro / Reduced-Form

## Purpose

Skeleton template for a standard applied microeconomics empirical paper targeting top economics journals. Fill in bracketed sections.

---

## Paper Structure

```latex
\documentclass[12pt]{article}
\usepackage[margin=1in]{geometry}
\usepackage{amsmath,amssymb}
\usepackage{natbib}
\bibliographystyle{chicago}
\usepackage{booktabs,tabularx}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{setspace}
\doublespacing

\title{[Title: Descriptive, 10-15 words, conveys main finding]}
\author{[Author Name]\thanks{[Affiliation]. Email: [email].
I thank [acknowledgments]. All errors are my own.}}
\date{\today}

\begin{document}
\maketitle

\begin{abstract}
[1-2 sentence motivation.] [1 sentence: what this paper does.] [1 sentence: identification strategy.] [1-2 sentences: main results with magnitudes.] [1 sentence: implications.]
% Target: 100-150 words
\end{abstract}

\textbf{JEL Codes}: [XXX, YYY, ZZZ]\\
\textbf{Keywords}: [keyword1, keyword2, keyword3, keyword4, keyword5]

%% ============================================================
\section{Introduction}
%% ============================================================

% Paragraph 1: Motivation + question
[Start with compelling fact, puzzle, or policy context. 2-3 sentences.]
[State research question clearly in 1-2 sentences.]

% Paragraph 2: What this paper does
In this paper, I estimate [the causal effect of X on Y] using [identification strategy].
I exploit [source of exogenous variation] to isolate [the effect of interest].
The data come from [data source], covering [N observations] over [time period].

% Paragraph 3: Preview of results
I find that [main result with magnitude and statistical significance].
This implies that [economic interpretation / benchmark].
[1-2 sentences on key robustness results or heterogeneity.]

% Paragraph 4: Contribution
This paper contributes to several strands of literature. First, [literature strand 1 with 2-3 citations].
Relative to this work, I [what you add]. Second, [literature strand 2 with 2-3 citations].
My contribution is [what you add]. [Optional: Third strand.]

% Paragraph 5: Roadmap
The rest of the paper is organized as follows. Section 2 describes [institutional background].
Section 3 presents the data. Section 4 outlines the empirical strategy.
Section 5 reports the main results. Section 6 presents robustness checks.
Section 7 concludes.

%% ============================================================
\section{Institutional Background}
%% ============================================================
% Only include if needed for identification
% Describe the policy/institution that generates your variation
% Include timeline of key events
% Visual timeline figure is helpful

[Describe the institutional context relevant to identification.]
[Policy change / natural experiment details.]
[Why this setting is useful for causal identification.]

%% ============================================================
\section{Data}
%% ============================================================

\subsection{Data Sources}
[Describe each dataset, how accessed, what it contains.]
[Describe how datasets are merged if multiple sources.]

\subsection{Sample Construction}
[Describe sample restrictions with justification.]
[Provide observation counts at each restriction step.]

\subsection{Variable Definitions}
[Define key variables: outcome, treatment, controls.]
[Discuss any measurement concerns.]

\subsection{Summary Statistics}
[Reference Table 1.]
[Highlight key statistics: means, variation, balance between groups.]

% Table 1: Summary Statistics
% Columns: Variable, Mean, SD, Min, Max, N
% Optional: separate columns for treated/control with difference test

%% ============================================================
\section{Empirical Strategy}
%% ============================================================

\subsection{Identification Strategy}
[State the identification strategy formally.]
[Explain the source of exogenous variation.]

The main estimating equation is:
\begin{equation}
Y_{it} = \alpha + \beta \cdot D_{it} + X_{it}'\gamma + \mu_i + \delta_t + \varepsilon_{it}
\label{eq:main}
\end{equation}
where $Y_{it}$ is [outcome] for [unit] $i$ in [period] $t$, $D_{it}$ is [treatment indicator],
$X_{it}$ is a vector of [controls], $\mu_i$ and $\delta_t$ are [unit] and [time] fixed effects,
and $\varepsilon_{it}$ is the error term. The coefficient of interest is $\beta$,
which captures [interpretation under identifying assumptions].

\subsection{Identifying Assumptions}
[State key assumptions formally.]
[Discuss testable implications.]
[Discuss untestable assumptions and why they are plausible.]

\subsection{Threats to Identification}
[Discuss potential violations and how you address them.]

%% ============================================================
\section{Results}
%% ============================================================

\subsection{Main Results}
[Reference Table 2 (main regression table).]
[Walk through columns: baseline → adding controls → preferred specification.]
[Interpret coefficient magnitude in economic terms.]
[Benchmark: compare to mean of dependent variable, prior literature, policy-relevant metric.]

\subsection{Event Study / Dynamic Effects}
[Reference Figure 1 (event study plot).]
[Discuss pre-trends (should be flat).]
[Discuss post-treatment dynamics.]

\subsection{Heterogeneity Analysis}
[Reference Table 3.]
[Pre-specified subgroup analyses.]
[Interpret: for whom is the effect larger/smaller?]

%% ============================================================
\section{Robustness}
%% ============================================================

\subsection{Alternative Specifications}
[Reference Table 4.]
[Different controls, functional forms, sample definitions.]

\subsection{Placebo and Falsification Tests}
[Reference Table 5 or Figure 2.]
[Placebo outcomes, placebo treatment timing, placebo treatment groups.]

\subsection{Sensitivity Analysis}
[Oster (2019) bounds: report δ.]
[Bandwidth/specification sensitivity for RDD.]
[Alternative estimators for staggered DID.]

%% ============================================================
\section{Conclusion}
%% ============================================================

[1-2 sentences: restate main finding.]
[1-2 sentences: policy implications.]
[1-2 sentences: limitations.]
[1-2 sentences: directions for future research.]
% Keep brief: 0.5-1 page

\bibliography{references}

%% ============================================================
% APPENDIX (for Online Appendix)
%% ============================================================
\appendix
\section{Additional Tables and Figures}
\section{Data Appendix}
\section{Proofs} % if applicable

\end{document}
```

---

## Table Skeleton: Main Results

```
Table 2: Effect of [Treatment] on [Outcome]
─────────────────────────────────────────────────────────
                        (1)        (2)        (3)        (4)
                        OLS        OLS        [ID]       [ID]
─────────────────────────────────────────────────────────
[Treatment]            [β̂]***    [β̂]***    [β̂]***    [β̂]**
                      ([se])     ([se])     ([se])     ([se])

[Key control 1]                   [β̂]**                [β̂]*
                                 ([se])                ([se])
─────────────────────────────────────────────────────────
Controls                No         Yes        No         Yes
Unit FE                 Yes        Yes        Yes        Yes
Time FE                 Yes        Yes        Yes        Yes
Observations          [N]        [N]        [N]        [N]
R² / First-stage F    [R²]       [R²]       [F]        [F]
Dep. Var. Mean        [mean]     [mean]     [mean]     [mean]
─────────────────────────────────────────────────────────
Notes: Standard errors clustered at [level] in parentheses.
* p<0.10, ** p<0.05, *** p<0.01. [Variable definitions.]
```

---

## Checklist Before Submission

### Content
- [ ] Abstract ≤ 150 words
- [ ] Introduction ≤ 5 pages
- [ ] Research question stated in first page
- [ ] Identification strategy clearly explained
- [ ] All assumptions stated
- [ ] Results interpreted in economic terms (not just statistical significance)
- [ ] Robustness checks address main threats
- [ ] Conclusion is brief and doesn't overstate findings

### Formatting
- [ ] JEL codes included
- [ ] Keywords included
- [ ] Tables have notes explaining all variables
- [ ] Stars defined: * 10%, ** 5%, *** 1%
- [ ] Standard errors in parentheses
- [ ] N and R² in every table
- [ ] Figures readable in grayscale
- [ ] References in Chicago Author-Date format

### Replication
- [ ] All code runs from raw data to final output
- [ ] README explains how to replicate
- [ ] Data sources documented with access instructions
- [ ] Software versions noted
