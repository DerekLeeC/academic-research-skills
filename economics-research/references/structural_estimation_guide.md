# Structural Estimation Guide — Methods & Best Practices

## Purpose

Reference guide for structural estimation methods in economics. Covers specification, identification, estimation, and validation for major classes of structural models.

---

## 1. Why Structural Estimation?

### When Reduced-Form Is Insufficient
- **Counterfactual analysis**: Need to simulate untried policies
- **Welfare**: Need to recover utility/welfare from observed choices
- **Multiple equilibria**: Need model to interpret which equilibrium is observed
- **General equilibrium**: Reduced-form estimates are partial equilibrium
- **Mechanism**: Need to quantify channels, not just total effect

### When Reduced-Form Is Preferred
- **Simple policy question**: Does X increase Y? By how much?
- **Credible identification**: Strong quasi-experimental design available
- **Minimal assumptions**: Want results robust to model misspecification
- **Transparency**: Assumptions easily stated and tested

### The Complementary View
- Reduced-form estimates **validate** structural models (do they match?)
- Structural models **interpret** reduced-form estimates (what mechanism?)
- Best papers often combine both approaches

---

## 2. Estimation Methods

### Maximum Likelihood Estimation (MLE)

**Setup**: Specify full likelihood L(θ|data) = Π_i f(data_i|θ)

**Advantages**:
- Efficient (achieves Cramér-Rao bound)
- Natural framework for testing (LR, Wald, LM tests)
- Handles complex data structures (censoring, selection, panel)

**Challenges**:
- Requires complete distributional specification
- Multiple local optima (use multiple starting values)
- Numerical integration for random effects (quadrature, simulation)

**Practical tips**:
- Start with simpler model, use estimates as starting values for complex model
- Check gradient is close to zero at solution
- Verify Hessian is negative definite
- Compare numerical and analytical gradients
- Use robust (sandwich) standard errors

### Generalized Method of Moments (GMM)

**Setup**: Population moment conditions E[g(data, θ₀)] = 0

**Two-step GMM**:
```
Step 1: θ̂₁ = argmin g_N(θ)' I g_N(θ)        (identity weight matrix)
Step 2: θ̂₂ = argmin g_N(θ)' Ŵ⁻¹ g_N(θ)     (optimal weight matrix from Step 1)
```

**Practical tips**:
- Start with identity or diagonal weighting matrix
- Report both one-step and two-step estimates
- CUE (continuously updated estimator) for better finite-sample properties
- J-test for overidentification
- Choose moments that are economically interpretable

### Simulated Method of Moments (SMM)

**When**: Moments cannot be computed analytically from model

**Procedure**:
```
1. Choose target moments m_data (from data)
2. For each θ candidate:
   a. Simulate model S times → compute m_sim(θ)
   b. Compute distance: [m_data - m_sim(θ)]' W [m_data - m_sim(θ)]
3. Minimize distance over θ
```

**Practical tips**:
- Use many simulation draws (S >> N) to reduce simulation noise
- Fix random seed for smooth objective function
- Use common random numbers across θ evaluations
- Choose moments with identifying power (not just goodness-of-fit)

### Indirect Inference

**Key idea**: Match parameters of an auxiliary model (not moments directly)

**Procedure** (Gourieroux, Monfort & Renault 1993):
```
1. Estimate auxiliary model β̂ on real data
2. For each θ, simulate data → estimate auxiliary model → β̃(θ)
3. Minimize: [β̂ - β̃(θ)]' W [β̂ - β̃(θ)]
```

**Advantage**: Auxiliary model can be easy to estimate (e.g., VAR, OLS)
**Choice of auxiliary model**: Should capture features the structural model aims to explain

### Bayesian Estimation

**Setup**: Posterior ∝ Likelihood × Prior

**MCMC algorithms**:
- **Metropolis-Hastings**: General purpose, requires tuning
- **Gibbs sampler**: When conditional posteriors have known forms
- **Hamiltonian MC**: Better mixing for complex posteriors (Stan)
- **Sequential MC (particle filter)**: For state-space models

**Practical tips for DSGE Bayesian estimation**:
- Use mode-finding first (posterior mode as starting value)
- Check convergence: trace plots, Gelman-Rubin R̂, effective sample size
- Report prior vs. posterior to show data informativeness
- Model comparison via marginal likelihood (Laplace approximation)

---

## 3. Identification in Structural Models

### Formal Identification

**Definition**: θ is identified if θ₁ ≠ θ₂ implies different model predictions.

**Types**:
- **Point identification**: Unique θ maps to data distribution
- **Set identification**: Multiple θ consistent with data → identified set
- **Partial identification**: Bounds on parameters/counterfactuals

### Common Identification Arguments

| Model Class | Identification Source |
|-------------|---------------------|
| Discrete choice (static) | Variation in choice sets, prices, characteristics |
| BLP demand | Price instruments (cost shifters), substitution patterns across markets |
| Auction models | Variation in number of bidders, reserve prices |
| Search models | Accepted wage distribution, unemployment duration |
| Dynamic discrete choice | Exclusion restrictions (variables affecting current payoff but not future) |
| DSGE | Cross-equation restrictions from theory |
| Production functions | Timing of input decisions (Olley-Pakes logic) |

### Identification Verification Tools
- **Sensitivity analysis**: How much do estimates change with small perturbations?
- **Monte Carlo**: Simulate data from known parameters → can you recover them?
- **Overidentification test**: J-test for GMM/SMM
- **Information matrix**: Check rank condition (is Hessian full rank?)

---

## 4. Model Validation

### Internal Validation
1. **In-sample fit**: Do model predictions match data moments (including untargeted ones)?
2. **Overidentification**: More moments than parameters → J-test
3. **Parameter plausibility**: Are estimated parameters in reasonable range?
4. **Comparative statics**: Do model predictions move in the right direction?

### External Validation
1. **Out-of-sample prediction**: Does model predict data from different period/market?
2. **Reduced-form comparison**: Do model-implied treatment effects match quasi-experimental estimates?
3. **Moment comparison**: Does model match moments not used in estimation?
4. **Policy validation**: If policy was later implemented, did model predict correctly?

### Reporting Best Practices
- Table of targeted moments (data vs. model)
- Table of untargeted moments (data vs. model)
- Comparison with reduced-form estimates
- Sensitivity of counterfactuals to parameter perturbations
- Monte Carlo evidence on estimator performance

---

## 5. Software Guide

### DSGE Models
| Software | Language | Strengths |
|----------|----------|-----------|
| **Dynare** | MATLAB/Octave | Industry standard, Bayesian estimation, perturbation |
| **RISE** | MATLAB | Regime-switching DSGE |
| **DSGE.jl** | Julia | Fast, modern |
| **dolo** | Python | Nonlinear solution methods |
| **QuantEcon** | Python/Julia | Heterogeneous agent models, educational |
| **HANK models** | Julia/MATLAB | Sequence space (Auclert et al.) |

### Industrial Organization
| Software | Language | Use |
|----------|----------|-----|
| **PyBLP** | Python | BLP demand estimation |
| **BLPestimatoR** | R | BLP demand |
| **Counterfactual** | MATLAB | Merger simulation |

### Auctions
| Software | Language | Use |
|----------|----------|-----|
| **GPVR** | MATLAB/R | First-price auction estimation |

### General Optimization
| Software | Language | Use |
|----------|----------|-----|
| **Optim.jl** | Julia | Fast general optimization |
| **NLopt** | C/Python/R/Julia | Nonlinear optimization (many algorithms) |
| **scipy.optimize** | Python | General optimization |
| **Knitro** | MATLAB/Python | Large-scale nonlinear |

---

## 6. Checklist for Structural Papers

### Before Estimation
- [ ] Economic model clearly specified (primitives, equilibrium)
- [ ] Identification argument stated formally
- [ ] Functional form assumptions justified
- [ ] Distributional assumptions justified (or robustness to alternatives)
- [ ] Data requirements matched to model

### During Estimation
- [ ] Multiple starting values tried
- [ ] Convergence verified (gradient ≈ 0, Hessian negative definite)
- [ ] Standard errors computed (delta method, bootstrap, or posterior)
- [ ] Monte Carlo simulation shows estimator recovers true parameters

### After Estimation
- [ ] Targeted moments fit well
- [ ] Untargeted moments fit reasonably
- [ ] Parameters in plausible range
- [ ] Reduced-form comparison (if available)
- [ ] Sensitivity analysis reported
- [ ] Counterfactuals clearly described
- [ ] Code available for replication
