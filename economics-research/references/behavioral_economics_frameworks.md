# Behavioral Economics Frameworks — Theory & Experimental Design Reference

## Purpose

Reference guide for behavioral economics theories, experimental paradigms, and design principles. Used by the behavioral_experimental_agent.

---

## 1. Core Behavioral Economics Models

### Prospect Theory (Kahneman & Tversky 1979)

**Key components**:
- **Reference dependence**: Utility defined over gains and losses from reference point
- **Loss aversion**: Losses loom larger than gains (λ ≈ 2.0-2.5)
- **Diminishing sensitivity**: Concave for gains, convex for losses
- **Probability weighting**: Overweight small probabilities, underweight large ones

**Value function**:
```
v(x) = x^α           if x ≥ 0
v(x) = -λ(-x)^β      if x < 0

Typical estimates: α ≈ β ≈ 0.88, λ ≈ 2.25
```

**Probability weighting function** (Prelec 1998):
```
w(p) = exp(-(-ln p)^α)
Typical: α ≈ 0.65
```

**Applications**: Insurance demand, asset pricing puzzles, labor supply, tax compliance, consumer choice

### Present Bias (Quasi-Hyperbolic Discounting)

**β-δ model** (Laibson 1997; O'Donoghue & Rabin 1999):
```
U_t = u_t + β Σ_{s>t} δ^{s-t} u_s

β = 1: standard exponential discounting
β < 1: present bias (overvalue immediate payoffs)
Typical: β ≈ 0.7-0.9, δ ≈ 0.95-0.99
```

**Sophistication spectrum**:
- **Naive**: β̂ = 1 (doesn't know they're present-biased)
- **Sophisticated**: β̂ = β (knows their bias)
- **Partial naiveté**: β < β̂ < 1 (underestimates bias)

**Applications**: Savings, exercise, procrastination, addiction, commitment devices

### Social Preferences

**Fehr-Schmidt (1999) inequity aversion**:
```
U_i = x_i - α_i max(x_j - x_i, 0) - β_i max(x_i - x_j, 0)

α: disutility from disadvantageous inequality (envy)
β: disutility from advantageous inequality (guilt)
Typical: α ≈ 0.5-1.0, β ≈ 0.25-0.5, with α > β
```

**Charness-Rabin (2002)**:
```
U_i(x_i, x_j) = (1 - ρ·r - σ·s - θ·q) x_i + (ρ·r + σ·s + θ·q) x_j
```

**Reciprocity** (Rabin 1993): Utility depends on perceived intentions

**Applications**: Wage setting, public goods, charitable giving, negotiations, contract design

### Limited Attention

**Rational inattention** (Sims 2003; Matějka & McKay 2015):
- Agents have limited information processing capacity
- Optimally allocate attention across decisions
- Implies: errors in choices, correlation between stakes and accuracy

**Salience theory** (Bordalo, Gennaioli & Shleifer 2012):
- Attention drawn to attributes that stand out (are salient)
- Context-dependent choice: which attribute is salient depends on comparison set

**Shrouded attributes** (Gabaix & Laibson 2006):
- Firms strategically shroud add-on costs
- Sophisticates exploit deals; naifs pay add-on prices

**Applications**: Consumer finance, tax salience, advertising, default options

### Overconfidence & Beliefs

**Types of overconfidence** (Moore & Healy 2008):
1. **Overestimation**: Thinking you're better than you are
2. **Overplacement**: Thinking you're better than others (better-than-average effect)
3. **Overprecision**: Excessive certainty in your beliefs (narrow confidence intervals)

**Confirmation bias**: Seeking/interpreting evidence that confirms prior beliefs

**Base rate neglect**: Underweighting prior probabilities relative to new evidence

**Applications**: Financial markets, entrepreneurship, forecasting, medical diagnosis

---

## 2. Experimental Design Principles

### Incentive Compatibility

**Induced value theory** (Smith 1976): To study economic behavior in the lab, you must:
1. **Monotonicity**: Subjects prefer more reward to less
2. **Salience**: Reward is tied to actions in the experiment
3. **Dominance**: Reward from experiment dominates any other motivations

**Incentive-compatible belief elicitation**:
| Method | Mechanism | When to Use |
|--------|-----------|-------------|
| Quadratic scoring rule | Minimize expected squared error | Continuous beliefs |
| Binary lottery | Choose between lottery and certainty | Probability beliefs |
| Matching probability | BDM-style for event probabilities | Simple probabilities |
| Proper scoring rules | General class | Various settings |

**Incentive-compatible preference elicitation**:
| Method | Mechanism | Measures |
|--------|-----------|----------|
| Multiple Price List (MPL) | Series of binary choices | Risk/time preferences |
| BDM (Becker-DeGroot-Marschak) | Random price mechanism | WTP/WTA |
| Convex Time Budget | Allocate tokens across time | Time preferences (β, δ) |
| Strategic uncertainty (games) | Real payoff from game | Social preferences |

### Randomization

**Between-subject**: Each subject sees one treatment
- Advantage: No order effects, no demand from within-subject comparison
- Disadvantage: More subjects needed, individual heterogeneity adds noise

**Within-subject**: Each subject sees all treatments
- Advantage: Controls for individual heterogeneity, more power
- Disadvantage: Order effects, demand effects, carryover

**Strategy method**: Subjects decide for all possible scenarios
- Advantage: Rich data per subject
- Disadvantage: May differ from "hot" (real-time) decisions

### Power Analysis for Experiments

**Key formula**:
```
N per arm ≈ 16 × (σ/MDE)²   (for 80% power, α = 0.05)
```

**For cluster randomized designs**:
```
N_effective = N_total / (1 + (m-1) × ICC)
where m = cluster size, ICC = intra-cluster correlation
```

**MDE benchmarks** (Cohen's d):
| d | Interpretation | Required N per arm (80% power) |
|---|---------------|-------------------------------|
| 0.2 | Small | ~400 |
| 0.5 | Medium | ~65 |
| 0.8 | Large | ~26 |

### Demand Effects & Deception

**Experimenter demand effects**: Subjects try to confirm what they think experimenter wants.

**Mitigation strategies**:
- Double-blind procedures
- Neutral framing (avoid loaded language)
- Multiple experimental sessions / labs
- Online experiments (reduced social pressure)
- Measure demand directly (hypothetical vs. incentivized)

**Deception policy**:
- Economics tradition: **no deception** (unlike psychology)
- If subjects learn experimenters deceive, future experiments compromised
- Use incomplete information (don't lie, but don't reveal everything)

---

## 3. Common Experimental Paradigms

### Public Goods Game
```
Setup: N players, each endowed with e tokens
Decision: Contribute g_i ∈ [0, e] to public good
Payoff: π_i = (e - g_i) + α · Σ g_j  where 1/N < α < 1

Nash equilibrium (selfish): g_i = 0
Social optimum: g_i = e
Typical lab result: ~40-60% of endowment contributed initially, declining over rounds
```

### Dictator Game
```
Setup: Player 1 (dictator) has endowment e
Decision: Give g ∈ [0, e] to Player 2
Payoff: Dictator gets e - g, Recipient gets g

Nash (selfish): g = 0
Typical lab result: ~20-30% of endowment given
```

### Ultimatum Game
```
Setup: Player 1 proposes split of e; Player 2 accepts or rejects
If accept: split as proposed. If reject: both get 0.

Nash (selfish): Proposer offers ε → 0, Responder accepts
Typical lab result: Proposers offer ~40%; offers below 20% rejected ~50% of time
```

### Trust Game (Berg, Dickhaut & McCabe 1995)
```
Setup: Player 1 sends s ∈ [0, e]; tripled to 3s; Player 2 returns r ∈ [0, 3s]
Payoffs: Player 1 gets e - s + r; Player 2 gets 3s - r

Nash (selfish): r = 0, so s = 0
Typical lab result: ~50% sent, ~33% returned
```

### Risk Elicitation: Holt-Laury MPL
```
10 decisions between:
  Option A: {p, $2.00; 1-p, $1.60}
  Option B: {p, $3.85; 1-p, $0.10}

p increases from 0.1 to 1.0 in steps of 0.1
Switch point → implied CRRA coefficient
Typical: Switch at row 5-6 → moderate risk aversion (CRRA ≈ 0.3-0.5)
```

### Time Preference Elicitation: Convex Time Budget (Andreoni & Sprenger 2012)
```
Allocate budget between:
  c_t tokens at time t (worth $1 each)
  c_{t+k} tokens at time t+k (worth $(1+r) each)

Vary: delay k, gross interest rate (1+r), background payments
Estimate: (β, δ) from allocation choices
```

---

## 4. Field Experiment Design

### Phases of a Field RCT

1. **Design phase** (3-6 months)
   - Define research question and theory of change
   - Power analysis and sample size determination
   - Identify implementing partner
   - Design intervention and measurement instruments
   - Write pre-analysis plan
   - IRB approval

2. **Baseline** (1-3 months)
   - Baseline survey (if panel design)
   - Administrative data collection
   - Randomization

3. **Implementation** (varies)
   - Treatment delivery
   - Compliance monitoring
   - Process data collection

4. **Endline** (1-3 months)
   - Endline survey
   - Administrative outcome data
   - Qualitative data (for mechanisms)

5. **Analysis** (3-6 months)
   - Follow pre-analysis plan
   - Report deviations from plan
   - Write paper

### Common Threats to Field Experiments

| Threat | Description | Mitigation |
|--------|-------------|------------|
| Attrition | Subjects drop out differentially | Track aggressively, Lee bounds, IPW |
| Non-compliance | Treated don't take up, control gets treated | ITT as primary, IV for LATE |
| Spillovers | Treatment affects control group | Buffer zones, market-level randomization |
| Hawthorne effect | Behavior change from being observed | Measure control group behavior too |
| John Henry effect | Control group tries harder | Blind to assignment if possible |
| Contamination | Information leakage between arms | Separate randomization clusters |
| Ethical concerns | Withholding beneficial treatment | Phase-in design, equipoise |

---

## 5. Nudge & Choice Architecture

### Nudge Toolkit (Thaler & Sunstein 2008)

| Nudge Type | Mechanism | Example |
|------------|-----------|---------|
| **Default options** | Status quo bias | Opt-out vs. opt-in retirement savings |
| **Simplification** | Reduce cognitive load | Pre-filled tax forms |
| **Social norms** | Conformity | "9 out of 10 neighbors recycle" |
| **Salience** | Attention | Calorie labels, energy bills |
| **Commitment devices** | Bind future self | StickK, savings locks |
| **Framing** | Reference dependence | Gain vs. loss framing |
| **Anchoring** | Anchor-and-adjust | Suggested donation amounts |
| **Reminders** | Limited attention | SMS appointment reminders |
| **Implementation intentions** | Planning fallacy | "When will you vote?" |

### EAST Framework (Behavioural Insights Team)
- **Easy**: Reduce friction, simplify, use defaults
- **Attractive**: Use salience, rewards, personalization
- **Social**: Use social norms, networks, commitments
- **Timely**: Prompt when most receptive, use deadlines

---

## 6. Pre-Registration & Transparency

### Where to Pre-Register
| Registry | Focus | URL |
|----------|-------|-----|
| AEA RCT Registry | Economics RCTs | socialscienceregistry.org |
| OSF | All social science | osf.io |
| AsPredicted | Quick pre-registration | aspredicted.org |
| EGAP | Governance, political economy | egap.org |
| ClinicalTrials.gov | Health | clinicaltrials.gov |

### What to Pre-Register
1. Research question and hypotheses
2. Experimental design and randomization
3. Primary outcome variables and measurement
4. Sample size and power calculation
5. Primary statistical specification
6. Secondary analyses and subgroups
7. Multiple testing correction strategy
8. How to handle attrition and non-compliance

### Reporting Deviations
- Clearly label pre-registered vs. exploratory analyses
- Explain any deviations from the plan
- Report all pre-registered analyses (even null results)
