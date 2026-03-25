# Structural Economics Paper Template

## Purpose

Skeleton template for a structural economics paper (IO, labor, trade, macro). Covers model specification, identification, estimation, and counterfactual analysis.

---

## Paper Structure

```latex
\documentclass[12pt]{article}
\usepackage[margin=1in]{geometry}
\usepackage{amsmath,amssymb,amsthm}
\usepackage{natbib}
\bibliographystyle{chicago}
\usepackage{booktabs,tabularx}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{setspace}
\doublespacing

\newtheorem{assumption}{Assumption}
\newtheorem{proposition}{Proposition}

\title{[Title: Conveys Model + Application]}
\author{[Author]\thanks{[Affiliation, email, acknowledgments.]}}
\date{\today}

\begin{document}
\maketitle

\begin{abstract}
[1 sentence: question.] [1 sentence: what kind of model.] [1 sentence: data and estimation.]
[1-2 sentences: key parameter estimates.] [1-2 sentences: counterfactual results and policy implications.]
\end{abstract}

\textbf{JEL Codes}: [XXX, YYY, ZZZ]\\
\textbf{Keywords}: [keyword1, keyword2, keyword3]

%% ============================================================
\section{Introduction}
%% ============================================================

% Paragraph 1: Question and why it needs structure
[What question do you answer? Why can't reduced-form alone answer it?]
[Motivate the need for a structural approach: counterfactuals, welfare, mechanisms.]

% Paragraph 2: What this paper does
I develop a model of [economic environment] and estimate it using [data].
The model features [key economic forces / frictions].
I estimate the model using [estimation method: MLE / GMM / SMM / Bayesian]
and use the estimated model to [counterfactual exercises].

% Paragraph 3: Key results
The main findings are threefold. First, [key parameter estimate and interpretation].
Second, [counterfactual result 1]. Third, [counterfactual result 2 / policy implication].

% Paragraph 4: Contribution
This paper contributes to [literature 1] by [contribution].
It also relates to [literature 2]. Methodologically, [if applicable].

% Paragraph 5: Roadmap
The paper proceeds as follows. Section 2 presents the model. Section 3 discusses
identification. Section 4 describes the data. Section 5 presents estimation results.
Section 6 conducts counterfactual analysis. Section 7 concludes.

%% ============================================================
\section{Model}
%% ============================================================

\subsection{Environment}
% Describe agents, timing, information structure
[There are $N$ [agents] indexed by $i$. Time is [discrete/continuous, finite/infinite horizon].]
[Information structure: complete/incomplete/asymmetric.]
[Market structure: competitive/oligopoly/search frictions.]

\subsection{Preferences}
[Agent $i$ has utility function:]
\begin{equation}
U_i = [utility function with parameters]
\end{equation}

\subsection{Technology / Constraints}
[Production function / budget constraint / participation constraint:]
\begin{equation}
[constraint equation]
\end{equation}

\subsection{Timing of Decisions}
\begin{enumerate}
\item [Period 1: agents observe... and choose...]
\item [Period 2: ...]
\item [...]
\end{enumerate}

\subsection{Equilibrium}
\begin{definition}
An equilibrium consists of [strategies/allocations/prices] such that:
\begin{enumerate}
\item [Agent optimization condition]
\item [Market clearing / consistency condition]
\item [Any additional conditions]
\end{enumerate}
\end{definition}

% Key model results
\begin{proposition}
[State key theoretical result that aids identification or interpretation.]
\end{proposition}

\subsection{Discussion of Model Assumptions}
[Discuss key simplifying assumptions and their implications.]
[Which assumptions are important for results? Which are for tractability?]

%% ============================================================
\section{Identification}
%% ============================================================

% This section is critical for structural papers

\subsection{Identification Strategy}
[Explain what variation in data identifies each structural parameter.]

\begin{assumption}[Identification Assumption 1]
[Formal statement]
\end{assumption}

[Discuss plausibility.]

\subsection{Identification Arguments}
[For each key parameter:]

\textbf{Parameter $\theta_1$ ([name]):} Identified by [source of variation].
Intuitively, [explain in plain language].

\textbf{Parameter $\theta_2$ ([name]):} Identified by [source of variation].

\subsection{Reduced-Form Evidence}
[Present reduced-form evidence that is consistent with model mechanisms.]
[This bridges structural and reduced-form approaches and builds credibility.]

%% ============================================================
\section{Data}
%% ============================================================

\subsection{Data Sources}
[Describe datasets.]

\subsection{Sample and Variable Construction}
[How model objects map to data variables.]

\subsection{Summary Statistics}
[Reference Table 1. Highlight moments relevant to estimation.]

%% ============================================================
\section{Estimation}
%% ============================================================

\subsection{Estimation Method}
[I estimate the model by [MLE / GMM / SMM / Bayesian estimation].]

% For GMM/SMM:
The parameter vector $\theta$ is estimated by minimizing:
\begin{equation}
\hat{\theta} = \arg\min_\theta \left[ m(\theta) - m^{data} \right]' W \left[ m(\theta) - m^{data} \right]
\end{equation}
where $m(\theta)$ is the vector of model-implied moments and $m^{data}$ is the
corresponding data moments. $W$ is [the optimal weighting matrix / identity / diagonal].

\subsection{Moments / Likelihood}
% Table: which moments target which parameters
\begin{table}[h]
\caption{Moments Used in Estimation}
\begin{tabular}{lll}
\toprule
Moment & Data Value & Targets Parameter \\
\midrule
[Moment 1] & [value] & $\theta_1$ \\
[Moment 2] & [value] & $\theta_2$ \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Computational Details}
[Solution algorithm for the model.]
[Optimization algorithm for estimation.]
[Number of simulation draws (if simulated).]
[Multiple starting values attempted.]

\subsection{Parameter Estimates}
[Reference Table 2: estimated parameters with standard errors.]
[Interpret each parameter economically.]

\subsection{Model Fit}
% Table: targeted moments (data vs. model)
[Reference Table 3: targeted moments fit.]

% Table: untargeted moments (data vs. model) — key for validation
[Reference Table 4: untargeted moments fit.]

\subsection{Comparison with Reduced-Form Estimates}
[Compare model-implied treatment effects with quasi-experimental estimates from literature.]
[This is a powerful validation exercise.]

%% ============================================================
\section{Counterfactual Analysis}
%% ============================================================

\subsection{Counterfactual 1: [Policy/Scenario Name]}
[Describe what changes in the counterfactual.]
[What is held fixed? What re-equilibrates?]
[Results in Table 5 / Figure X.]
[Economic interpretation.]

\subsection{Counterfactual 2: [Policy/Scenario Name]}
[Same structure.]

\subsection{Sensitivity of Counterfactuals}
[How sensitive are counterfactual results to key parameter values?]
[Report range of counterfactual outcomes for plausible parameter range.]

%% ============================================================
\section{Conclusion}
%% ============================================================

[Restate main findings.]
[Policy implications from counterfactuals.]
[Limitations: what the model abstracts from.]
[Future directions.]

\bibliography{references}

\appendix
\section{Proofs}
\section{Computational Appendix}
\section{Additional Results}
\section{Monte Carlo Evidence}

\end{document}
```

---

## Structural Paper Checklist

### Model
- [ ] Economic environment clearly described
- [ ] All assumptions stated explicitly
- [ ] Equilibrium defined formally
- [ ] Key theoretical results proven or cited
- [ ] Simplifying assumptions discussed honestly

### Identification
- [ ] Each parameter's identification source stated
- [ ] Identification assumptions formalized
- [ ] Reduced-form supporting evidence provided
- [ ] Clear mapping: data variation → structural parameter

### Estimation
- [ ] Method justified (why MLE/GMM/SMM/Bayesian?)
- [ ] Moments chosen are informative (not just goodness-of-fit)
- [ ] Standard errors properly computed
- [ ] Multiple starting values used
- [ ] Convergence diagnostics reported

### Validation
- [ ] Targeted moments fit reported
- [ ] Untargeted moments fit reported (critical!)
- [ ] Comparison with reduced-form estimates (if available)
- [ ] Monte Carlo shows estimator works (optional but good)

### Counterfactuals
- [ ] Clearly described: what changes, what's held fixed
- [ ] Welfare implications computed
- [ ] Sensitivity analysis reported
- [ ] Lucas critique addressed (are behavioral parameters stable?)

### Replication
- [ ] Model solution code provided
- [ ] Estimation code provided
- [ ] Counterfactual simulation code provided
- [ ] Data or instructions to obtain data
