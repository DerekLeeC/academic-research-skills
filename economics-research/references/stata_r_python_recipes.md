# Stata / R / Python Code Recipes for Economics Research

## Purpose

Ready-to-use code templates for common econometric analyses. Each recipe includes Stata, R, and Python implementations where available.

---

## 1. Difference-in-Differences

### Basic 2×2 DID

**Stata:**
```stata
* Basic DID
reg Y treated##post X1 X2, robust

* With fixed effects
reghdfe Y treated_post, absorb(unit year) cluster(state)
```

**R:**
```r
library(fixest)
# Basic DID with two-way FE
did <- feols(Y ~ treated_post | unit + year, data = df, cluster = "state")
summary(did)
```

**Python:**
```python
import linearmodels as lm
mod = lm.PanelOLS.from_formula('Y ~ treated_post + EntityEffects + TimeEffects', data=df)
res = mod.fit(cov_type='clustered', cluster_entity=True)
```

### Staggered DID (Callaway-Sant'Anna)

**Stata:**
```stata
ssc install csdid, replace
csdid Y X1 X2, ivar(unit) time(year) gvar(first_treat) method(dripw)
csdid_plot
```

**R:**
```r
library(did)
att <- att_gt(yname = "Y", tname = "year", idname = "unit",
              gname = "first_treat", xformla = ~ X1 + X2, data = df)
ggdid(att)
aggte(att, type = "dynamic")  # event study aggregation
aggte(att, type = "simple")    # overall ATT
```

### Event Study

**Stata:**
```stata
* Generate relative time dummies
gen rel_time = year - first_treat
forval i = -5/10 {
    gen D`i' = (rel_time == `i')
}
drop D_1  // base period

reghdfe Y D_5-D_0 D1-D10 X1 X2, absorb(unit year) cluster(state)
coefplot, keep(D*) vertical yline(0) xline(5.5, lp(dash))
```

**R:**
```r
library(fixest)
es <- feols(Y ~ sunab(first_treat, year) + X1 + X2 | unit + year,
            data = df, cluster = "state")
iplot(es, main = "Event Study")
```

---

## 2. Instrumental Variables

### 2SLS

**Stata:**
```stata
* Basic 2SLS
ivregress 2sls Y X1 X2 (endog = instrument), first robust

* With fixed effects
ivreghdfe Y X1 X2 (endog = instrument), absorb(unit year) cluster(state) first
```

**R:**
```r
library(fixest)
iv <- feols(Y ~ X1 + X2 | unit + year | endog ~ instrument, data = df, cluster = "state")
summary(iv, stage = 1:2)
```

**Python:**
```python
from linearmodels.iv import IV2SLS
mod = IV2SLS.from_formula('Y ~ 1 + X1 + X2 + [endog ~ instrument]', data=df)
res = mod.fit(cov_type='robust')
```

### Weak Instrument Diagnostics

**Stata:**
```stata
* Effective F-statistic (Montiel Olea & Pflueger)
weakivtest

* Anderson-Rubin test
ivregress 2sls Y (endog = instrument), robust
estat endogenous  // Durbin-Wu-Hausman
```

**R:**
```r
library(ivDiag)
ivDiag(Y ~ endog | X1 + X2 | instrument, data = df, bootstrap = TRUE)
```

---

## 3. Regression Discontinuity

### Sharp RD

**Stata:**
```stata
ssc install rdrobust, replace
rdrobust Y X, c(0) kernel(triangular) bwselect(mserd)
rdplot Y X, c(0) nbins(20 20) ci(95)

* Manipulation test
rddensity X, c(0)

* Covariate balance
rdrobust covariate X, c(0)
```

**R:**
```r
library(rdrobust)
rd <- rdrobust(Y, X, c = 0)
summary(rd)
rdplot(Y, X, c = 0)

library(rddensity)
rddensity(X, c = 0)
```

**Python:**
```python
from rdrobust import rdrobust, rdplot, rdbwselect
rd = rdrobust(Y, X, c=0)
print(rd)
rdplot(Y, X, c=0)
```

### Fuzzy RD

**Stata:**
```stata
rdrobust Y X, c(0) fuzzy(D)
```

**R:**
```r
rdrobust(Y, X, c = 0, fuzzy = D)
```

---

## 4. Synthetic Control

**Stata:**
```stata
ssc install synth, replace
synth Y Y(1985) Y(1990) X1 X2, trunit(3) trperiod(1995) fig
```

**R:**
```r
# Classic Synth
library(Synth)
dataprep.out <- dataprep(foo = df, predictors = c("X1", "X2"),
                          dependent = "Y", unit.variable = "unit",
                          time.variable = "year", treatment.identifier = 3,
                          controls.identifier = c(1,2,4:10),
                          time.predictors.prior = 1985:1994,
                          time.optimize.ssr = 1985:1994,
                          time.plot = 1985:2005)
synth.out <- synth(dataprep.out)
path.plot(synth.out, dataprep.out)

# Augmented Synthetic Control
library(augsynth)
asyn <- augsynth(Y ~ D, unit, year, df, progfunc = "Ridge")
summary(asyn)
plot(asyn)
```

---

## 5. Panel Data Essentials

### Fixed Effects

**Stata:**
```stata
* Standard FE
xtreg Y X1 X2, fe robust
* or
reghdfe Y X1 X2, absorb(unit) cluster(state)

* Two-way FE
reghdfe Y X1 X2, absorb(unit year) cluster(state)

* Hausman test (FE vs RE)
xtreg Y X1 X2, fe
estimates store fe
xtreg Y X1 X2, re
estimates store re
hausman fe re
```

**R:**
```r
library(fixest)
# One-way FE
fe1 <- feols(Y ~ X1 + X2 | unit, data = df, cluster = "state")

# Two-way FE
fe2 <- feols(Y ~ X1 + X2 | unit + year, data = df, cluster = "state")

# Compare
etable(fe1, fe2, se = "cluster")
```

### Clustering

**Stata:**
```stata
* Cluster at state level
reghdfe Y X1 X2, absorb(unit year) cluster(state)

* Two-way clustering
reghdfe Y X1 X2, absorb(unit year) cluster(state year)

* Wild cluster bootstrap (few clusters)
boottest X1, cluster(state) reps(999) seed(12345)
```

**R:**
```r
library(fixest)
# Single cluster
feols(Y ~ X1 + X2 | unit + year, data = df, cluster = "state")

# Two-way clustering
feols(Y ~ X1 + X2 | unit + year, data = df, cluster = ~ state + year)

# Wild bootstrap
library(fwildclusterboot)
boottest(model, param = "X1", clustid = "state", B = 999)
```

---

## 6. Matching & Propensity Score

**Stata:**
```stata
* PSM
teffects psmatch (Y) (D X1 X2 X3, probit), atet nn(1) caliper(0.05)
tebalance summarize

* IPW
teffects ipw (Y) (D X1 X2 X3, logit), atet

* AIPW (doubly robust)
teffects aipw (Y X1 X2) (D X1 X2 X3, logit), atet

* Entropy balancing
ssc install ebalance
ebalance D X1 X2 X3, targets(1 2)  // balance on means and variances
```

**R:**
```r
# MatchIt
library(MatchIt)
m <- matchit(D ~ X1 + X2 + X3, data = df, method = "nearest",
             distance = "glm", caliper = 0.05)
summary(m)
m.data <- match.data(m)
lm(Y ~ D + X1 + X2 + X3, data = m.data, weights = weights)

# Entropy balancing
library(WeightIt)
w <- weightit(D ~ X1 + X2 + X3, data = df, method = "ebal")
library(survey)
d <- svydesign(~1, weights = w$weights, data = df)
svyglm(Y ~ D + X1 + X2 + X3, design = d)
```

---

## 7. Shift-Share Instruments

**Stata:**
```stata
ssc install bartik_weight, replace
bartik_weight, z(bartik_instrument) weightstub(share_) ///
  x(treatment) y(outcome) controls(controls) weight_var(pop)

* ssaggregate (shock-level approach)
ssc install ssaggregate, replace
ssaggregate Y X controls [aw=weight], n(industry) s(share_stub_) t(year) l(location)
ivreg2 y_n (x_n = g_n) [aw=s_n], robust
```

**R:**
```r
library(ShiftShareSE)
# Exposure-robust SEs
reg <- lm(Y ~ bartik + controls, data = df, weights = pop)
summary_ss(reg, X = "bartik", shares = share_matrix, alpha = 0.05)
```

---

## 8. Oster (2019) Bounds

**Stata:**
```stata
ssc install psacalc, replace
* After running your regression:
reg Y X1 X2 X3
psacalc delta X1  // proportional selection δ
psacalc beta X1, delta(1) rmax(1.3)  // identified set with δ=1, R²max=1.3·R̃²
```

**R:**
```r
# Using the coefficient_stability package or manual calculation
# Manual Oster bounds
r_tilde <- summary(lm(Y ~ D + X1 + X2, data=df))$r.squared
r_max <- min(1.3 * r_tilde, 1)
beta_tilde <- coef(lm(Y ~ D + X1 + X2, data=df))["D"]
beta_short <- coef(lm(Y ~ D, data=df))["D"]
# δ = 1 bound
beta_star <- beta_tilde - (beta_short - beta_tilde) * (r_max - r_tilde) / (r_tilde - 0)
```

---

## 9. Productivity Estimation (TFP)

**Stata:**
```stata
ssc install prodest, replace
* Levinsohn-Petrin
prodest log_Y, free(log_L) proxy(log_M) state(log_K) met(lp) id(firm) t(year)

* Ackerberg-Caves-Frazer
prodest log_Y, free(log_L) proxy(log_M) state(log_K) met(lp) acf id(firm) t(year)

* Recover TFP
predict tfp, residual
```

**R:**
```r
library(prodest)
mod <- prodestLP(Y = df$log_Y, fX = df$log_L, sX = df$log_K,
                  pX = df$log_M, idvar = df$firm, timevar = df$year)
summary(mod)
omega <- mod@omega.hat  # TFP estimates
```

---

## 10. Machine Learning for Causal Inference

### Double/Debiased ML

**R:**
```r
library(DoubleML)
library(mlr3learners)

data_ml <- DoubleMLData$new(df, y_col = "Y", d_cols = "D",
                             x_cols = c("X1", "X2", "X3"))
ml_l <- lrn("regr.ranger", num.trees = 500)
ml_m <- lrn("regr.ranger", num.trees = 500)
dml <- DoubleMLPLR$new(data_ml, ml_l, ml_m)
dml$fit()
dml$summary()
```

**Python:**
```python
from econml.dml import DML, LinearDML
from sklearn.ensemble import RandomForestRegressor, RandomForestClassifier

est = LinearDML(model_y=RandomForestRegressor(n_estimators=500),
                model_t=RandomForestClassifier(n_estimators=500))
est.fit(Y, T, X=X, W=W)
print(est.effect(X))
print(est.effect_interval(X))
```

### Causal Forest

**R:**
```r
library(grf)
cf <- causal_forest(X = as.matrix(df[, c("X1","X2","X3")]),
                     Y = df$Y, W = df$D, num.trees = 2000)
# ATE
average_treatment_effect(cf)
# Heterogeneous effects
tau_hat <- predict(cf)$predictions
hist(tau_hat)
# Variable importance
variable_importance(cf)
```

---

## 11. Useful Stata Commands

```stata
* Summary statistics table
estpost summarize Y X1 X2 X3
esttab using "sumstats.tex", cells("mean sd min max N") replace

* Balance table
iebaltab Y X1 X2 X3, grpvar(treated) save("balance.xlsx") replace

* Export regression tables
eststo: reghdfe Y X1, absorb(unit year) cluster(state)
eststo: reghdfe Y X1 X2, absorb(unit year) cluster(state)
eststo: reghdfe Y X1 X2 X3, absorb(unit year) cluster(state)
esttab using "results.tex", se star(* 0.10 ** 0.05 *** 0.01) ///
  stats(N r2 FE_unit FE_year, label("Observations" "R²" "Unit FE" "Year FE")) ///
  replace booktabs

* Coefficient plot
coefplot est1 est2 est3, keep(X1) xline(0)
```

## 12. Useful R Packages for Economics

```r
# Core econometrics
library(fixest)        # Fast FE estimation, IV, cluster SE
library(sandwich)      # Robust/cluster SEs
library(lmtest)        # Hypothesis tests
library(ivreg)         # IV regression
library(plm)           # Panel data

# Causal inference
library(did)           # Callaway-Sant'Anna DID
library(rdrobust)      # RD design
library(Synth)         # Synthetic control
library(augsynth)      # Augmented synthetic control
library(MatchIt)       # Matching
library(WeightIt)      # Weighting (IPW, entropy balancing)
library(grf)           # Causal forest

# Tables and visualization
library(modelsummary)  # Regression tables
library(fixest)        # etable() for tables
library(ggplot2)       # Visualization
library(patchwork)     # Combine plots
```
