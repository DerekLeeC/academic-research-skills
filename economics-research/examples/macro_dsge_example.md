# Example: Macro DSGE — Monetary Policy with Financial Frictions

## Purpose

Worked example showing how the economics-research skill designs methodology for a macroeconomic modeling question.

---

## User Prompt

> "Help me build a DSGE model to study how financial frictions amplify the transmission of monetary policy shocks."

---

## Phase 1: Model Design

### Macro-Finance Agent — Model Specification

**Research Question**: How do financial frictions in the banking sector amplify the real effects of monetary policy shocks, and does the amplification vary with the state of bank balance sheets?

**Model Class**: Medium-scale New Keynesian DSGE with financial intermediaries.

**Baseline**: Gertler & Kiyotaki (2010) / Gertler & Karadi (2011) framework, extended with occasionally binding leverage constraints for state-dependent amplification.

### Model Environment

**Agents**:
1. **Households**: Infinitely-lived, supply labor, save via deposits
2. **Financial intermediaries (banks)**: Collect deposits, make loans to firms, subject to leverage constraint
3. **Intermediate goods firms**: Produce using capital and labor, subject to price stickiness (Calvo)
4. **Capital goods firms**: Convert output into new capital, subject to adjustment costs
5. **Retailers**: Bundle intermediate goods, set prices (Calvo pricing)
6. **Central bank**: Sets interest rate via Taylor rule
7. **Government**: Fiscal authority, lump-sum taxes

**Key frictions**:
1. **Financial friction**: Moral hazard problem → banks face endogenous leverage constraint
2. **Nominal rigidity**: Calvo price stickiness
3. **Capital adjustment costs**: Convex investment adjustment costs

### Core Model Equations

**Household**:
```
max E_0 Σ β^t [log(C_t) - χ·L_t^{1+φ}/(1+φ)]
s.t. C_t + D_t = W_t·L_t + R_t^d·D_{t-1} + T_t
```

**Financial intermediary (key block)**:
```
Balance sheet: Q_t·S_t = N_t + D_t
  where Q_t = price of capital, S_t = loans, N_t = net worth, D_t = deposits

Net worth evolution:
  N_t = (R_t^k - R_t^d)·Q_{t-1}·S_{t-1} + R_t^d·N_{t-1}
  where R_t^k = return on capital, R_t^d = deposit rate

Moral hazard constraint (Gertler-Karadi):
  V_t ≥ θ·Q_t·S_t
  where V_t = franchise value, θ = fraction banker can divert

Implies leverage constraint:
  Q_t·S_t / N_t ≤ φ_t (endogenous leverage ratio)

Credit spread:
  E_t[R_{t+1}^k] - R_t^d = f(leverage, aggregate state)
```

**Firms (standard NK)**:
```
Production: Y_t = A_t · K_t^α · L_t^{1-α}
Calvo pricing: fraction (1-ξ) reset prices each period
Phillips curve: π_t = β·E_t[π_{t+1}] + κ·mc_t
```

**Monetary policy (Taylor rule)**:
```
R_t = ρ_R·R_{t-1} + (1-ρ_R)·[R* + φ_π·(π_t - π*) + φ_y·ŷ_t] + ε_t^m
```

**Shocks**: Monetary (ε^m), technology (ε^a), financial (ε^σ — net worth shock)

### Key Mechanism

```
Monetary tightening (↑R)
  → ↓ Asset prices (↓Q)
  → ↓ Bank net worth (N = equity = assets - liabilities)
  → Leverage constraint tightens
  → ↑ Credit spreads
  → ↓ Investment, ↓ Output
  → Further ↓ Asset prices (amplification via "financial accelerator")
```

**Without financial frictions**: Monetary shock → standard NK channel (intertemporal substitution + cost channel)
**With financial frictions**: Additional amplification through bank balance sheet channel

---

## Phase 2: Identification & Estimation Strategy

### Identification

**Parameters identified from**:

| Parameter | Identified By | Data Source |
|-----------|--------------|-------------|
| β (discount) | Steady-state real rate | FRED (3-month T-bill / CPI) |
| α (capital share) | Labor income share | NIPA |
| ξ (Calvo price) | Frequency of price changes | Micro price data (Nakamura & Steinsson) |
| φ_π, φ_y (Taylor rule) | Interest rate response to inflation/output | Estimated from macro time series |
| θ (divertable fraction) | Steady-state leverage ratio + credit spread | Flow of Funds, FRED spreads |
| σ (shock volatilities) | Variance of observables | Macro time series |

### Estimation Method: Bayesian Estimation

**Why Bayesian**:
- Standard for medium-scale DSGE (Smets-Wouters tradition)
- Handles model misspecification through priors
- Full posterior distribution for parameters and impulse responses
- Model comparison via marginal likelihood

**Observable variables** (7 observables, typical):
1. Real GDP growth
2. Consumption growth
3. Investment growth
4. Hours worked
5. Inflation (GDP deflator)
6. Federal funds rate
7. Credit spread (BAA - 10yr Treasury)

**Data**: US quarterly, 1985Q1-2023Q4, from FRED.

**Priors**:

| Parameter | Prior | Distribution | Mean | SD | Source |
|-----------|-------|-------------|------|-----|--------|
| ξ (Calvo) | Calvo price stickiness | Beta | 0.75 | 0.10 | Micro evidence |
| φ_π (Taylor: inflation) | Taylor rule inflation | Normal | 1.50 | 0.25 | Literature |
| φ_y (Taylor: output) | Taylor rule output | Normal | 0.125 | 0.05 | Literature |
| ρ_R (interest smoothing) | Interest rate smoothing | Beta | 0.75 | 0.10 | Literature |
| θ (divertable fraction) | Moral hazard | Beta | 0.38 | 0.05 | Gertler-Karadi |
| 100·σ_m (monetary shock) | MP shock size | InvGamma | 0.10 | 0.05 | Diffuse |

**Software**: Dynare 5.x (MATLAB/Octave)

```matlab
% Dynare .mod file structure
var Y C I K L R pi spread N Q;
varexo e_m e_a e_sigma;

parameters beta alpha xi phi_pi phi_y rho_R theta ...;

model;
  // Household Euler
  1/C = beta * E(1/C(+1) * R / pi(+1));
  // ...
  // Bank leverage constraint
  // ...
  // Taylor rule
  R = rho_R * R(-1) + (1-rho_R) * (Rss + phi_pi*(pi-piss) + phi_y*yhat) + e_m;
end;

estimated_params;
  xi, beta_pdf, 0.75, 0.10;
  phi_pi, normal_pdf, 1.50, 0.25;
  // ...
end;

varobs dy dc di hours pi R spread;

estimation(datafile=us_data, mh_replic=100000, mh_nblocks=2,
           mh_jscale=0.3, mode_compute=4, bayesian_irf);
```

---

## Phase 3: Validation & Counterfactuals

### Robustness Design Agent — Model Validation

**Internal validation**:
1. **Moment matching**: Compare model-implied and data moments (output volatility, investment volatility, spread volatility, correlation structure)
2. **Impulse responses**: Compare model IRFs to monetary shock with SVAR evidence (Gertler & Karadi 2015 proxy SVAR using high-frequency identification)
3. **Variance decomposition**: What fraction of output variance is due to financial shocks vs. monetary vs. technology?

**Sensitivity analysis**:
1. Vary θ (moral hazard parameter) — key for amplification magnitude
2. Vary ξ (price stickiness) — affects real effects of monetary policy
3. Alternative Taylor rules (include credit spread, financial stability mandate)
4. Compare log-linear vs. nonlinear solution (occasionally binding constraint)

**Model comparison**:
1. Model with financial frictions vs. standard NK (no frictions) → marginal likelihood comparison
2. Gertler-Karadi vs. BGG (different financial friction specification) → which fits data better?

### Counterfactual Exercises

**Counterfactual 1: No financial frictions (θ = 0)**
- Remove bank moral hazard → no leverage constraint
- Compare impulse responses: how much smaller is the output response to monetary shock?
- Quantify amplification ratio: σ_Y(with frictions) / σ_Y(without frictions)

**Counterfactual 2: Macroprudential policy**
- Add countercyclical capital requirement: θ_t = θ̄ - φ_N·(N_t - N̄)
- Question: Does macroprudential policy reduce output volatility? At what cost to credit provision?

**Counterfactual 3: State-dependent monetary policy**
- Taylor rule responds to credit spreads: R_t = ... + φ_s·(spread_t - spread̄)
- Question: Should the central bank "lean against the wind" of financial imbalances?

**Counterfactual 4: Great Recession simulation**
- Feed in estimated sequence of financial shocks (2007Q4-2009Q2)
- Decompose output decline: how much due to financial shock vs. monetary vs. technology?
- Counterfactual: what if the Fed had cut rates faster?

---

## Phase 4: Economics Methodology Brief

```markdown
## Economics Methodology Brief (EMB)

**Research Question**: How do financial frictions in the banking sector amplify the transmission of monetary policy shocks?

**Model**:
- Class: Medium-scale New Keynesian DSGE with financial intermediaries
- Key friction: Bank moral hazard (Gertler-Kiyotaki/Gertler-Karadi)
- Mechanism: Monetary shock → asset prices → bank net worth → leverage constraint → credit spread → investment → amplification
- Solution: 1st-order perturbation (Dynare) + occasionally binding constraint (for nonlinear analysis)

**Estimation**:
- Method: Bayesian (MCMC, 100,000 draws, 2 chains)
- Observables: GDP growth, consumption growth, investment growth, hours, inflation, FFR, credit spread
- Data: US quarterly, 1985Q1-2023Q4
- Software: Dynare 5.x (MATLAB)

**Validation**:
- Compare IRFs with proxy SVAR evidence (Gertler-Karadi 2015)
- Moment matching (targeted + untargeted)
- Marginal likelihood comparison: financial frictions vs. standard NK

**Counterfactuals**:
1. No financial frictions → quantify amplification
2. Macroprudential policy → optimal capital requirements
3. Credit-spread-augmented Taylor rule → "lean against the wind"
4. Great Recession decomposition

**JEL Codes**: E32, E44, E52, G21

**Literature Positioning**: Extends Gertler-Karadi (2011) with state-dependent amplification (occasionally binding leverage constraint) and updated Bayesian estimation through 2023. Contributes to the "lean vs. clean" debate in monetary policy by quantifying the interaction between conventional monetary policy and macroprudential tools.
```
