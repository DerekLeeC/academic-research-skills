# Causal Inference Toolkit — Methods Reference for Economics

## Purpose

Quick-reference guide for causal inference methods commonly used in economics. Covers assumptions, diagnostics, common pitfalls, and key citations for each method.

---

## 1. Difference-in-Differences (DID)

### Canonical Setup
- **Setting**: Binary treatment, two groups, two periods (extendable to staggered)
- **Estimand**: ATT (Average Treatment Effect on the Treated)
- **Key assumption**: Parallel trends

### Modern DID (Staggered Treatment)

**Problem with TWFE**: Goodman-Bacon (2021) decomposition shows TWFE is a weighted average of all 2×2 DID comparisons, including "bad" comparisons (already-treated as control). With heterogeneous treatment effects, weights can be negative.

**Solutions**:

| Estimator | Key Idea | Package (Stata) | Package (R) |
|-----------|----------|-----------------|-------------|
| Callaway & Sant'Anna (2021) | Group-time ATT, aggregate | `csdid` | `did` |
| Sun & Abraham (2021) | Interaction-weighted | `eventstudyinteract` | `sunab` (fixest) |
| de Chaisemartin & D'Haultfœuille (2020) | Robust to heterogeneous TE | `did_multiplegt` | `DIDmultiplegt` |
| Borusyak, Jaravel & Spiess (2024) | Imputation | `did_imputation` | `didimputation` |
| Gardner (2022) | Two-stage DID | `did2s` | `did2s` |
| Arkhangelsky et al. (2021) | Synthetic DID | `sdid` | `synthdid` |

### Diagnostics Checklist
- [ ] Event study plot with pre-treatment coefficients
- [ ] Joint F-test of pre-treatment coefficients = 0
- [ ] Bacon decomposition (for staggered)
- [ ] Placebo treatment dates
- [ ] Alternative control groups

### Key Citations
- Angrist & Pischke (2009), Ch. 5
- Goodman-Bacon (2021), "Difference-in-Differences with Variation in Treatment Timing"
- Roth et al. (2023), "What's Trending in Difference-in-Differences?"

---

## 2. Instrumental Variables (IV)

### Canonical Setup
- **Setting**: Endogenous X, instrument Z that affects Y only through X
- **Estimand**: LATE (Local Average Treatment Effect for compliers)
- **Key assumptions**: Relevance, exclusion restriction, independence, monotonicity

### Weak Instruments

| Test | Threshold | Citation |
|------|-----------|----------|
| First-stage F | > 10 (rule of thumb) | Staiger & Stock (1997) |
| Effective F | Context-dependent | Montiel Olea & Pflueger (2013) |
| Anderson-Rubin | Weak-IV robust CI | Anderson & Rubin (1949) |
| tF procedure | Weak-IV robust | Lee et al. (2022) |

### Common Instruments in Economics
| Domain | Instrument | For | Citation |
|--------|-----------|-----|----------|
| Education | Quarter of birth | Schooling | Angrist & Krueger (1991) |
| Institutions | Settler mortality | Institutions | Acemoglu et al. (2001) |
| Trade | Gravity-predicted trade | Trade openness | Frankel & Romer (1999) |
| Immigration | Shift-share (past enclaves) | Immigration | Card (2001) |
| Conflict | Rainfall | Economic shocks | Miguel et al. (2004) |
| Family | Twin births | Family size | Angrist & Evans (1998) |
| Crime | Judge leniency | Incarceration | Kling (2006) |
| Finance | Analyst coverage | Information | Yu (2008) |

### Key Citations
- Angrist & Pischke (2009), Ch. 4
- Andrews, Stock & Sun (2019), "Weak Instruments in IV Regression"

---

## 3. Regression Discontinuity Design (RDD)

### Canonical Setup
- **Setting**: Treatment assigned by threshold on running variable X
- **Estimand**: Local ATE at the cutoff
- **Key assumption**: Continuity of potential outcomes at cutoff

### Implementation (rdrobust)

```stata
* Sharp RD
rdrobust Y X, c(0) kernel(triangular) bwselect(mserd)
rdplot Y X, c(0) nbins(20 20)

* Fuzzy RD
rdrobust Y X, c(0) fuzzy(D)

* Manipulation test
rddensity X, c(0)
```

```r
library(rdrobust)
rdrobust(Y, X, c = 0)
rdplot(Y, X, c = 0)
rddensity(X, c = 0)
```

### Diagnostics
- [ ] McCrary/Cattaneo-Jansson-Ma density test
- [ ] Covariate balance at cutoff
- [ ] Bandwidth sensitivity (50%, 75%, 125%, 150%)
- [ ] Polynomial order sensitivity
- [ ] Placebo cutoffs
- [ ] Donut-hole RD

### Key Citations
- Cattaneo, Idrobo & Titiunik (2020), "A Practical Introduction to Regression Discontinuity Designs"
- Calonico, Cattaneo & Titiunik (2014), "Robust Nonparametric Confidence Intervals for RDD"
- Lee & Lemieux (2010), "Regression Discontinuity Designs in Economics"

---

## 4. Synthetic Control Method (SCM)

### Canonical Setup
- **Setting**: Single treated unit, multiple donor units, treatment at known time
- **Estimand**: Treatment effect for the treated unit
- **Key assumption**: Synthetic control reproduces counterfactual trajectory

### Implementation

```stata
synth Y Y(pre1) Y(pre2) X1 X2, trunit(1) trperiod(T) fig
```

```r
library(Synth)
synth.out <- synth(dataprep.out)

# Or augmented SCM
library(augsynth)
ascm <- augsynth(Y ~ D, unit, time, data)
```

### Inference
- Placebo tests (in-space): apply to each donor → permutation p-value
- Pre-treatment RMSPE ratio: treated / average placebo
- Conformal inference: Chernozhukov et al. (2021)

### Key Citations
- Abadie, Diamond & Hainmueller (2010)
- Abadie (2021), "Using Synthetic Controls: Feasibility, Data Requirements, and Methodological Aspects"

---

## 5. Bunching Estimation

### Canonical Setup
- **Setting**: Kink or notch in policy schedule creates bunching in distribution
- **Estimand**: Behavioral response elasticity

### Implementation

```stata
* bunching package
bunching Y, kink(threshold) binwidth(width) degree(7) bandwidth(bandwidth)
```

```r
library(bunching)
bun <- bunching(z_vector = Y, kink = threshold, binwidth = width)
```

### Key Citations
- Saez (2010), "Do Taxpayers Bunch at Kink Points?"
- Chetty, Friedman, Olsen & Pistaferri (2011)
- Kleven (2016), "Bunching"

---

## 6. Shift-Share (Bartik) Instruments

### Canonical Setup
- **Setting**: B_i = Σ_k s_{ik} · g_k (exposure shares × shocks)
- **Identification**: Either shocks g_k are exogenous (Borusyak et al.) or shares s_{ik} are exogenous (Goldsmith-Pinkham et al.)

### Implementation

```stata
* Shift-share IV
bartik_weight, z(shift_share) weightstub(share_) x(treatment) y(outcome) ///
  controls(controls) weight_var(pop)

* ssaggregate (Borusyak et al.)
ssaggregate Y X controls [aw=weight], n(industry) s(share_) t(year) l(location)
```

### Key Citations
- Goldsmith-Pinkham, Sorkin & Swift (2020)
- Borusyak, Hull & Jaravel (2022)
- Adão, Kolesár & Morales (2019)

---

## 7. Event Study Design

### Modern Best Practices

```stata
* Using fixest (R) or reghdfe (Stata) for basic event study
reghdfe Y ib(-1).relative_time, absorb(unit year) cluster(state)
event_plot, stub(relative_time) ci(95)

* Sun-Abraham estimator
eventstudyinteract Y lead* lag*, cohort(first_treat) control_cohort(never_treat) absorb(unit year)

* Callaway-Sant'Anna
csdid Y, ivar(unit) time(year) gvar(first_treat) method(dripw)
```

```r
library(fixest)
feols(Y ~ sunab(first_treat, year) | unit + year, data)

library(did)
att_gt <- att_gt(yname="Y", tname="year", idname="unit", gname="first_treat", data=data)
```

### Key Citations
- Sun & Abraham (2021)
- Callaway & Sant'Anna (2021)
- Roth (2022), "Pretest with Caution"

---

## 8. Matching & Propensity Score Methods

### Methods Comparison

| Method | Assumption | Advantage | Disadvantage |
|--------|-----------|-----------|--------------|
| PSM (nearest-neighbor) | CIA, overlap | Intuitive, flexible | Curse of dimensionality |
| IPW | CIA, overlap | Uses all data | Extreme weights |
| AIPW (doubly robust) | CIA, overlap | Consistent if either model correct | More complex |
| CEM (coarsened exact) | CIA, overlap | Exact balance | May drop many obs |
| Entropy balancing | CIA, overlap | Automatic balance | Hainmueller (2012) |

### Implementation

```stata
* Propensity score matching
teffects psmatch (Y) (D X1 X2 X3, probit), atet nn(1)

* IPW
teffects ipw (Y) (D X1 X2 X3, probit), atet

* AIPW (doubly robust)
teffects aipw (Y X1 X2) (D X1 X2 X3, probit), atet
```

```r
library(MatchIt)
m.out <- matchit(D ~ X1 + X2 + X3, data = data, method = "nearest")

library(WeightIt)
w.out <- weightit(D ~ X1 + X2 + X3, data = data, method = "ebal")
```

### Key Citations
- Imbens (2004), "Nonparametric Estimation of Average Treatment Effects Under Exogeneity"
- Abadie & Imbens (2006), "Large Sample Properties of Matching Estimators"

---

## 9. Selection Models

### Heckman Two-Step
```stata
heckman Y X1 X2, select(D = Z1 Z2 X1) twostep
```

### Lee (2009) Bounds
```stata
leebounds Y D, tight(X)
```

### Key Citations
- Heckman (1979)
- Lee (2009), "Training, Wages, and Sample Selection"

---

## 10. Machine Learning for Causal Inference

### Methods

| Method | Use Case | Reference |
|--------|----------|-----------|
| LASSO for control selection | High-dimensional controls | Belloni, Chernozhukov & Hansen (2014) |
| Double/Debiased ML | Causal inference with ML nuisance | Chernozhukov et al. (2018) |
| Causal Forest | Heterogeneous treatment effects | Wager & Athey (2018) |
| Matrix completion | Synthetic control generalization | Athey et al. (2021) |
| Bayesian Additive Regression Trees | Treatment effects | Hill (2011) |

### Implementation

```r
library(DoubleML)
dml <- DoubleMLPLR$new(data_ml, ml_l, ml_m, "Y", "D")
dml$fit()

library(grf)
cf <- causal_forest(X, Y, D)
average_treatment_effect(cf)
```

```python
from econml.dml import DML
from sklearn.ensemble import RandomForestRegressor
est = DML(model_y=RandomForestRegressor(), model_t=RandomForestRegressor())
est.fit(Y, T, X, W)
```

### Key Citations
- Athey & Imbens (2019), "Machine Learning Methods That Economists Should Know About"
- Chernozhukov et al. (2018), "Double/Debiased Machine Learning"
