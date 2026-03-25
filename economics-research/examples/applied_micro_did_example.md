# Example: Applied Micro DID — Minimum Wage and Employment

## Purpose

Worked example showing how the economics-research skill designs methodology for a classic applied micro question using difference-in-differences.

---

## User Prompt

> "Help me design a study to estimate the effect of minimum wage increases on teen employment in the US."

---

## Phase 1: Identification Design

### Econometrics Advisor Agent — Method Landscape

**Research Question (Causal)**: What is the causal effect of state-level minimum wage increases on teen (16-19) employment rates?

**Ideal Experiment**: Randomly assign different minimum wage levels across identical labor markets and measure employment responses.

**Identification Challenge**: States that raise the minimum wage differ systematically from those that don't (economic conditions, political preferences, labor market tightness). Naive OLS of employment on minimum wage is biased by these confounders.

**Candidate Strategies**:

1. **Two-way FE / DID**: Compare employment changes in states that raise MW vs. states that don't, controlling for state and year FE. Classic approach (Neumark & Wascher vs. Dube, Lester & Reich).

2. **Border-county pairs** (Dube, Lester & Reich 2010): Compare adjacent counties across state borders where one side raised MW. Controls for local economic conditions that smooth across borders.

3. **Event study / Staggered DID**: Exploit timing variation in state MW increases. Modern estimators (Callaway-Sant'Anna) handle heterogeneous effects.

4. **Bunching**: If minimum wage is binding, look at bunching in the wage distribution at the new MW level (Cengiz, Dube, Lindner & Zipperer 2019).

**Recommended Strategy**: Staggered DID with event study + border-county pair robustness.

---

### Identification Strategy Agent — Blueprint

**Proposed Strategy: Staggered DID with Event Study**

**Source of exogenous variation**: Timing of state minimum wage increases, conditional on state and time fixed effects.

**Formal setup**:
```
Y_st = α_s + δ_t + Σ_k β_k · D_{s,t-k} + X_st'γ + ε_st

where:
  Y_st = teen employment-to-population ratio in state s, year-quarter t
  α_s = state fixed effects
  δ_t = year-quarter fixed effects
  D_{s,t-k} = indicator for k periods since MW increase in state s
  X_st = time-varying state controls (GDP growth, unemployment)
```

**Key assumptions**:
1. **Parallel trends** — Absent MW increases, teen employment would have evolved similarly in treatment and control states. Testable via pre-treatment event study coefficients.
2. **No anticipation** — Employment doesn't adjust before the MW increase takes effect. Partially testable (check for pre-trends starting 1-2 quarters before).
3. **SUTVA** — MW increase in one state doesn't affect employment in other states. Could be violated via cross-border spillovers.

**Estimand**: ATT — average effect of MW increases on teen employment for states that raised their MW.

**Threats to identification**:

| Threat | Severity | Mitigation |
|--------|----------|------------|
| Differential pre-trends | High | Event study plot; pre-trend F-test |
| Endogenous timing (states raise MW when economy is strong) | Medium | Control for state GDP growth; Callaway-Sant'Anna |
| Staggered DID bias (Goodman-Bacon) | Medium | Use Callaway-Sant'Anna estimator |
| Spillovers across borders | Low-Medium | Border-county pair analysis |
| Federal MW confound | Medium | Exclude periods of federal increases |
| Compositional changes | Low | Use employment-to-population ratio (not employment level) |

**Alternative strategies**:
1. **Border-county pairs** (Dube et al. 2010): Stronger control for local conditions, but fewer observations and requires county-level data
2. **Bunching** (Cengiz et al. 2019): Directly measures job loss vs. redistribution in wage distribution; complementary evidence

---

## Phase 2: Data & Estimation

### Data Strategy Agent

**Recommended Primary Dataset**: Current Population Survey (CPS) — Monthly / MORG (Merged Outgoing Rotation Groups)

- **Access**: Public via IPUMS-CPS (cps.ipums.org)
- **Coverage**: Monthly, 1979-present, all US states
- **Key variables**: Employment status, age, state, hourly wage (MORG), demographics
- **Unit**: Individual-level, aggregated to state-quarter

**Supplementary Datasets**:
1. **Minimum wage data**: Vaghul & Zipperer (2016) state MW database; or manually collect from DOL
2. **State controls**: BLS (LAUS for unemployment), BEA (state GDP)
3. **County-level** (for border analysis): QCEW (Quarterly Census of Employment and Wages)

**Variable construction**:

| Variable | Source | Construction |
|----------|--------|-------------|
| Teen emp-pop ratio | CPS | employed/(population aged 16-19), by state-quarter |
| MW increase indicator | MW database | 1 if state MW increased in quarter t |
| Log real MW | MW database + CPI | log(nominal MW / CPI), deflated to constant dollars |
| State unemployment | LAUS | Quarterly average |
| State GDP growth | BEA | Year-over-year quarterly growth |

**Sample**: State-quarter observations, 2000Q1-2023Q4, 50 states + DC. ~4,800 state-quarter observations.

### Econometrics Advisor — Estimation Plan

**Primary estimator**: Callaway & Sant'Anna (2021) — group-time ATT

```stata
* Stata implementation
csdid teen_emprate state_controls, ivar(state) time(yq) gvar(first_mw_increase) method(dripw)
csdid_plot, title("Effect of MW Increase on Teen Employment")

* Aggregate to event study
csdid_stats event, window(-8 12)
```

**Standard errors**: Cluster at state level (50 clusters — borderline for cluster-robust; supplement with wild cluster bootstrap)

```stata
* Wild cluster bootstrap
boottest treatment_indicator, cluster(state) reps(999) seed(12345)
```

**Alternative estimators for comparison**:
1. TWFE (traditional, for comparison only — known to be biased)
2. Sun & Abraham (2021)
3. Synthetic DID (Arkhangelsky et al. 2021)

---

## Phase 3: Robustness & Evaluation

### Robustness Design Agent

**Robustness Blueprint**:

#### Threat 1: Parallel Trends Violation
**Tests**:
1. Event study plot — pre-treatment coefficients should be zero (Figure 1)
2. Joint F-test of all pre-treatment coefficients = 0
3. Roth (2022) pre-trend sensitivity: how large could a pre-trend be that we'd fail to detect?

#### Threat 2: Staggered DID Bias
**Tests**:
1. Bacon decomposition — examine weights on each 2×2 comparison
2. Compare TWFE vs. Callaway-Sant'Anna vs. Sun-Abraham — if similar, bias is small
3. Report only "clean" comparisons (never-treated as control)

#### Threat 3: Endogenous Timing
**Tests**:
1. Regress MW increase on lagged economic conditions — should be insignificant
2. Include state-specific linear trends
3. Control for state GDP growth and unemployment rate

#### Threat 4: Border Spillovers
**Tests**:
1. Border-county pair analysis (Dube et al. 2010 design)
2. Restrict control group to non-adjacent states

#### Standard Checks
| # | Check | Purpose |
|---|-------|---------|
| 1 | Placebo outcome: prime-age (25-54) employment | MW shouldn't affect non-minimum-wage workers |
| 2 | Placebo treatment date: fake MW increases 2 years early | Should show no effect |
| 3 | Dose-response: use log real MW instead of binary | Effect should scale with MW magnitude |
| 4 | Exclude federal MW increase years | Avoid confound |
| 5 | Trim sample to states with MW > federal MW | Focus on binding state-level variation |
| 6 | Alternative age group: 20-24 | Smaller but nonzero effect expected |
| 7 | Oster (2019) bounds with δ = 1 | Selection on unobservables |

**Table Plan**:
- Table 1: Summary statistics
- Table 2: Main results (Callaway-Sant'Anna ATT)
- Table 3: Comparison across estimators (TWFE, C-S, S-A, SDID)
- Table 4: Robustness — alternative specifications and samples
- Table 5: Placebo and falsification tests
- Table 6: Heterogeneity (by state income level, urbanization, industry composition)
- Figure 1: Event study plot
- Figure 2: Bacon decomposition
- Appendix: Dose-response, border-county analysis, Oster bounds

---

## Phase 4: Synthesis — Economics Methodology Brief

```markdown
## Economics Methodology Brief (EMB)

**Research Question**: What is the causal effect of state-level minimum wage increases on teen employment in the US?

**Identification Strategy**:
- Method: Staggered DID (Callaway-Sant'Anna 2021)
- Assumptions: Parallel trends, no anticipation, SUTVA
- Threats: Endogenous timing, staggered bias, border spillovers
- Notation: Y_st = α_s + δ_t + Σ_k β_k·D_{s,t-k} + X_st'γ + ε_st

**Data Strategy**:
- Datasets: CPS-MORG (IPUMS), state MW database, BLS/BEA state controls
- Variables: Teen emp-pop ratio, MW increase timing, log real MW
- Sample: 50 states + DC, 2000Q1-2023Q4, quarterly
- Period: 2000-2023

**Estimation Plan**:
- Estimator: Callaway-Sant'Anna group-time ATT
- Standard errors: State-level clustering + wild bootstrap
- Inference: Report both asymptotic and bootstrap CIs

**Robustness Plan**:
1. Event study pre-trends + F-test
2. Bacon decomposition
3. Estimator comparison (TWFE, C-S, S-A, SDID)
4. Placebo outcomes (prime-age employment)
5. Placebo treatment dates
6. Dose-response (log real MW)
7. Border-county pairs
8. Oster bounds

**JEL Codes**: J23, J38, J21

**Literature Positioning**: Contributes to the Neumark-Wascher vs. Dube-Lester-Reich debate using modern staggered DID estimators on updated data through 2023. Novelty: applies credibility revolution methods (Callaway-Sant'Anna) to a classic question.
```
