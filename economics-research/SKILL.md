---
name: economics-research
description: "Economics and management research methodology skill. 8-agent team specialized for empirical and theoretical economics research across all subfields. Covers econometric methodology selection, causal identification strategy design, data source guidance, robustness testing, structural estimation, behavioral/experimental economics, policy evaluation, and macroeconomic modeling. Integrates with deep-research and academic-paper for full pipeline support. Triggers on: economics research, econometrics, causal inference, identification strategy, DID, difference-in-differences, instrumental variable, IV, regression discontinuity, RDD, synthetic control, panel data, time series, structural estimation, DSGE, behavioral economics, experimental economics, RCT, policy evaluation, applied micro, applied microeconomics, macro, macroeconomics, labor economics, industrial organization, IO, public economics, development economics, finance, financial economics, trade, international trade, urban economics, health economics, environmental economics, 經濟學研究, 計量經濟學, 因果推論, 工具變數, 斷點迴歸, 雙重差分, 面板數據, 結構估計, 行為經濟學, 實驗經濟學, 政策評估, 總體經濟, 勞動經濟, 產業組織, 公共經濟, 發展經濟, 財務金融, 國際貿易."
metadata:
  version: "1.0"
  last_updated: "2026-03-24"
  depends_on: "deep-research, academic-paper"
---

# Economics Research — Economics & Management Research Methodology Skill

A domain-specific 8-agent team for rigorous economics and management research. Covers the full spectrum from applied microeconomics to macroeconomic modeling, with deep expertise in causal inference, structural estimation, experimental design, and policy evaluation. Designed to complement the general-purpose `deep-research` and `academic-paper` skills with economics-specific methodology, data source knowledge, and disciplinary conventions.

## Quick Start

**Minimal command:**
```
Help me design an identification strategy for studying the effect of minimum wage on employment
帮我设计一个研究最低工资对就业影响的因果识别策略
```

**With specific method:**
```
I want to use a DID approach to study the impact of a new environmental regulation on firm productivity
Review my IV strategy: I'm using rainfall as an instrument for agricultural income
```

**Structural/Macro:**
```
Help me set up a DSGE model with financial frictions for monetary policy analysis
Guide me through estimating a BLP demand model for the automobile market
```

**Data guidance:**
```
What datasets are available for studying intergenerational mobility in China?
I need panel data on firm-level trade for developing countries
```

---

## Trigger Conditions

### Trigger Keywords

**English**: economics research, econometrics, causal inference, identification strategy, DID, difference-in-differences, instrumental variable, IV, regression discontinuity, RDD, synthetic control, panel data, time series, structural estimation, DSGE, behavioral economics, experimental economics, RCT, policy evaluation, applied micro, macro, labor economics, industrial organization, public economics, development economics, finance, health economics, environmental economics, trade, urban economics, bunching, shift-share, Bartik instrument, event study, triple difference, propensity score, matching, selection model, Heckman, GMM, maximum likelihood, Bayesian estimation, field experiment, lab experiment, mechanism design, auction theory, game theory, general equilibrium, partial equilibrium

**中文**: 经济学研究, 计量经济学, 因果推论, 因果识别, 工具变量, 断点回归, 双重差分, 合成控制法, 面板数据, 时间序列, 结构估计, 行为经济学, 实验经济学, 政策评估, 应用微观, 宏观经济, 劳动经济, 产业组织, 公共经济, 发展经济学, 财务金融, 健康经济学, 环境经济学, 国际贸易, 城市经济学

### Does NOT Trigger

| Scenario | Use Instead |
|----------|-------------|
| General research (non-economics) | `deep-research` |
| Writing a paper (not methodology design) | `academic-paper` |
| Reviewing a paper | `academic-paper-reviewer` |
| Full research-to-paper pipeline | `academic-pipeline` |

### Integration with Existing Skills

This skill acts as a **domain-specific methodology layer** that enhances the general pipeline:

```
economics-research (methodology design)
  → deep-research (literature search + synthesis)
    → academic-paper (paper writing)
      → academic-paper-reviewer (peer review)
```

**Handoff to deep-research**: Produces an Economics Methodology Brief (EMB) containing identification strategy, data requirements, estimation approach, and robustness plan. This supplements the RQ Brief with economics-specific technical details.

**Handoff to academic-paper**: The EMB informs the `structure_architect_agent` on economics-standard paper structure and the `draft_writer_agent` on discipline-specific conventions (e.g., results table formatting, star notation for significance levels).

---

## Agent Team (8 Agents)

| # | Agent | Role | Phase |
|---|-------|------|-------|
| 1 | `econometrics_advisor_agent` | Econometric method selection, estimation strategy, statistical inference guidance | Phase 1 |
| 2 | `identification_strategy_agent` | Causal identification design: DID, IV, RDD, synthetic control, bunching, shift-share, selection models | Phase 1 |
| 3 | `data_strategy_agent` | Data source recommendation, variable construction, sample design, data cleaning protocols | Phase 2 |
| 4 | `robustness_design_agent` | Robustness check design: placebo tests, sensitivity analysis, alternative specifications, falsification tests | Phase 3 |
| 5 | `structural_modeling_agent` | Structural model specification, estimation (MLE, GMM, simulated methods), counterfactual analysis | Phase 1-3 (structural track) |
| 6 | `behavioral_experimental_agent` | Experimental design (lab, field, natural), behavioral model integration, incentive compatibility, pre-analysis plans | Phase 1-2 (experimental track) |
| 7 | `policy_evaluation_agent` | Policy evaluation frameworks, welfare analysis, cost-benefit analysis, general equilibrium effects, political economy | Phase 3 |
| 8 | `macro_finance_agent` | Macroeconomic modeling (DSGE, VAR, SVAR), asset pricing, financial frictions, calibration vs. estimation | Phase 1-3 (macro track) |

---

## Mode Selection Guide

| Your Situation | Recommended Mode |
|----------------|-----------------|
| Have a research question, need identification strategy | `identification` |
| Need help choosing econometric methods | `methods` |
| Need data source recommendations | `data` |
| Want full methodology design (ID + data + estimation + robustness) | `full` |
| Working on structural/theoretical model | `structural` |
| Designing an experiment (lab/field/natural) | `experimental` |
| Evaluating a policy intervention | `policy` |
| Macro/finance modeling question | `macro` |
| Want guided Socratic dialogue on methodology | `socratic` |

Not sure? Start with `full` — it covers all aspects and will route to specialized agents as needed.

---

## Orchestration Workflow

### Full Mode (4 Phases)

```
User: "Help me design a study on [economics topic]"
     |
=== Phase 1: IDENTIFICATION DESIGN ===
     |
     |-> [econometrics_advisor_agent] -> Method Landscape
     |   - Map research question to econometric paradigm
     |   - Identify candidate estimation strategies
     |   - Flag common pitfalls for this question type
     |
     |-> [identification_strategy_agent] -> Identification Strategy Blueprint
     |   - Primary identification strategy with formal assumptions
     |   - Threats to identification + mitigation plan
     |   - Alternative strategies for comparison
     |   - Formal notation (potential outcomes / DAG)
     |
     |   ** User confirmation before Phase 2 **
     |
=== Phase 2: DATA & ESTIMATION ===
     |
     |-> [data_strategy_agent] -> Data Strategy
     |   - Recommended datasets (with access instructions)
     |   - Variable construction guide
     |   - Sample selection criteria
     |   - Missing data & measurement error considerations
     |
     |-> [econometrics_advisor_agent] -> Estimation Plan
     |   - Specific estimator selection (with justification)
     |   - Standard error clustering strategy
     |   - Inference considerations (finite sample, multiple testing)
     |   - Software implementation notes (Stata/R/Python)
     |
=== Phase 3: ROBUSTNESS & EVALUATION ===
     |
     |-> [robustness_design_agent] -> Robustness Blueprint
     |   - Mandatory robustness checks for this design
     |   - Placebo / falsification tests
     |   - Sensitivity analysis plan
     |   - Bounds analysis (if applicable)
     |
     |-> [policy_evaluation_agent] -> Policy Implications (if applicable)
     |   - Welfare analysis framework
     |   - External validity assessment
     |   - Policy counterfactuals
     |
=== Phase 4: SYNTHESIS ===
     |
     +-> Compile Economics Methodology Brief (EMB)
         - Complete methodology package
         - Ready for handoff to deep-research or academic-paper
         - Includes Stata/R/Python code skeletons
```

### Structural Track

```
User: "Help me estimate a structural model of [topic]"
     |
     |-> [structural_modeling_agent] -> Model Specification
     |   - Economic model (primitives, equilibrium concept)
     |   - Identification argument
     |   - Estimation strategy (MLE, GMM, simulation)
     |
     |-> [data_strategy_agent] -> Data Requirements
     |   - Moments to match / likelihood to construct
     |   - Data sources for calibrated parameters
     |
     |-> [robustness_design_agent] -> Model Validation
     |   - Over-identification tests
     |   - Out-of-sample prediction
     |   - Sensitivity to functional form assumptions
     |   - Counterfactual exercises
     |
     +-> Compile Structural EMB
```

### Experimental Track

```
User: "Help me design an experiment on [topic]"
     |
     |-> [behavioral_experimental_agent] -> Experimental Design
     |   - Treatment arms and control
     |   - Randomization strategy
     |   - Power analysis / sample size calculation
     |   - Pre-analysis plan outline
     |   - IRB considerations
     |
     |-> [data_strategy_agent] -> Data Collection Plan
     |   - Survey instrument design
     |   - Administrative data linkage
     |   - Outcome variable measurement
     |
     |-> [robustness_design_agent] -> Analysis Plan
     |   - Primary specification
     |   - Heterogeneity analysis
     |   - Attrition / compliance handling
     |   - Multiple hypothesis testing correction
     |
     +-> Compile Experimental EMB
```

### Macro Track

```
User: "Help me build a macro model for [topic]"
     |
     |-> [macro_finance_agent] -> Model Design
     |   - Model class selection (DSGE, VAR, heterogeneous agent, etc.)
     |   - Key frictions and mechanisms
     |   - Equilibrium concept and solution method
     |
     |-> [macro_finance_agent] -> Calibration & Estimation
     |   - Calibrated vs. estimated parameters
     |   - Data sources for calibration targets
     |   - Estimation method (Bayesian, SMM, IRF matching)
     |
     |-> [robustness_design_agent] -> Model Validation
     |   - Impulse response analysis
     |   - Moment matching diagnostics
     |   - Sensitivity to calibration choices
     |   - Comparison with reduced-form evidence
     |
     +-> Compile Macro EMB
```

---

## Socratic Mode

When the user is unsure about their methodology or wants guided thinking, activate Socratic mode. The `econometrics_advisor_agent` leads the dialogue.

**Socratic dialogue flow:**
1. **Research question clarity** — "What causal effect are you trying to estimate? What is the ideal experiment you wish you could run?"
2. **Identification challenge** — "What is the source of exogenous variation? Why can't you just run OLS?"
3. **Data feasibility** — "What data exists to implement this? What are the measurement challenges?"
4. **Threats & alternatives** — "What would invalidate your strategy? Is there a better design?"
5. **Contribution framing** — "How does this advance our understanding beyond existing work?"

---

## Economics Methodology Brief (EMB) — Output Schema

The EMB is the primary output of this skill, designed for handoff to `deep-research` and `academic-paper`.

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `research_question` | string | Precise causal/descriptive question |
| `identification_strategy` | object | `{method, assumptions, threats, notation}` |
| `data_strategy` | object | `{datasets, variables, sample, period}` |
| `estimation_plan` | object | `{estimator, standard_errors, inference}` |
| `robustness_plan` | list[object] | Each: `{test_name, purpose, specification}` |
| `code_skeleton` | object | `{stata, r, python}` — implementation starters |
| `literature_positioning` | string | How this contributes to the existing literature |

### Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `structural_model` | object | Model primitives, equilibrium, estimation (if structural) |
| `experimental_design` | object | Treatment, randomization, power analysis (if experimental) |
| `macro_model` | object | Model class, frictions, solution method (if macro) |
| `welfare_analysis` | object | Welfare framework, counterfactuals (if policy) |
| `subfield` | string | Economics subfield classification |
| `jel_codes` | list[string] | JEL classification codes |

---

## Cross-Cutting Principles

### 1. Credibility Revolution Standards

All identification strategies are evaluated against modern credibility revolution standards:
- **Design-based inference**: Emphasize research design over statistical adjustment
- **Transparency**: Pre-registration, pre-analysis plans, replication packages
- **Robust inference**: Cluster-robust SEs, randomization inference, bounds
- **Honest reporting**: Report null results, discuss limitations, avoid p-hacking

### 2. Evidence Hierarchy for Economics

| Level | Design | Internal Validity |
|-------|--------|-------------------|
| 1 | Randomized Controlled Trial (lab/field) | Highest |
| 2 | Natural experiment with strong first stage | High |
| 3 | Difference-in-Differences with parallel trends | High (if trends hold) |
| 4 | Regression Discontinuity (sharp) | High (local) |
| 5 | Instrumental Variables | High (if exclusion holds) |
| 6 | Regression Discontinuity (fuzzy) | Moderate-High |
| 7 | Synthetic Control | Moderate |
| 8 | Matching / Propensity Score | Moderate |
| 9 | Panel FE / Selection models | Moderate (selection on observables/unobservables) |
| 10 | Cross-sectional OLS | Low |

### 3. Disciplinary Conventions

- **Citation style**: Chicago Author-Date (dominant in economics), APA 7.0 as fallback
- **Paper structure**: Introduction → Literature → Model/Theory → Data → Empirical Strategy → Results → Robustness → Conclusion
- **Tables**: Regression tables with standard errors in parentheses, significance stars (*, **, ***), N and R² reported
- **Figures**: Clean, grayscale-friendly, with confidence intervals
- **JEL codes**: Required for all working papers and journal submissions
- **Replication**: Code + data availability statement mandatory

### 4. Top Journal Standards

This skill's methodology advice targets the standards of top economics journals:
- **Top 5**: AER, Econometrica, JPE, QJE, REStud
- **Top field journals**: JF, RFS, JoF (finance); JLE, JOLE (labor); RAND, JIE (IO); JPubE (public); JDE, AEJ:Applied (development); JME, AEJ:Macro (macro); JEEA, EJ (European)
- **Working paper series**: NBER, CEPR, IZA, CESifo

---

## Failure Paths

| Code | Scenario | Recovery |
|------|----------|----------|
| E1 | No credible identification strategy exists | Acknowledge; suggest descriptive/correlational framing or structural approach |
| E2 | Required data is inaccessible | Suggest alternative datasets or proxy variables; discuss feasibility constraints |
| E3 | Sample size too small for desired design | Power analysis; suggest alternative designs or data pooling strategies |
| E4 | Identification assumptions likely violated | Propose bounds analysis, sensitivity analysis, or alternative design |
| E5 | Research question is not well-defined | Activate Socratic mode to refine |
| E6 | Method mismatch (e.g., DID without staggered treatment) | Redirect to appropriate method; explain why original choice doesn't fit |
| E7 | Structural model not identified | Discuss additional data/exclusion restrictions/functional form assumptions |
