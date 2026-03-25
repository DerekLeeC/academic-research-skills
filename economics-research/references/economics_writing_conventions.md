# Economics Writing Conventions — Discipline-Specific Style Guide

## Purpose

Style and convention guide specific to economics academic writing. Complements the general APA 7.0 guide with economics-specific practices.

---

## Paper Structure

### Standard Empirical Economics Paper

```
1. Introduction (20-25% of paper)
   - Motivation: Why does this question matter?
   - Research question: What do you estimate?
   - Preview of approach: How do you identify the effect?
   - Preview of results: What do you find?
   - Contribution: How does this advance the literature?
   - Related literature (often at end of Introduction)
   - Roadmap paragraph (optional, brief)

2. Institutional Background / Context (if needed)
   - Policy details, historical context
   - Only include what's needed for identification

3. Data (10-15%)
   - Data sources
   - Sample construction (transparent)
   - Summary statistics table
   - Variable definitions

4. Empirical Strategy (15-20%)
   - Identification strategy
   - Formal specification
   - Threats to identification
   - How you address those threats

5. Results (20-25%)
   - Main results
   - Interpretation (economic significance, not just statistical)
   - Magnitude benchmarking

6. Robustness (10%)
   - Alternative specifications
   - Placebo / falsification tests
   - Sensitivity analysis
   - Heterogeneity analysis

7. Conclusion (5-10%)
   - Summary of findings
   - Policy implications
   - Limitations
   - Directions for future research

Appendix (Online)
   - Additional tables and figures
   - Data appendix
   - Proofs (if theoretical section)
```

### Key Differences from Other Social Sciences

| Feature | Economics | Other Social Sciences |
|---------|----------|----------------------|
| Literature review | Integrated into Introduction (1-2 paragraphs) | Separate section |
| Hypothesis statement | Usually implicit in research question | Explicit H1, H2, H3 |
| Results presentation | Regression tables with star notation | Various formats |
| Citation style | Chicago Author-Date | APA, MLA, etc. |
| Mathematical notation | Expected and common | Less common |
| Abstract length | 100-150 words | 150-300 words |
| Theory section | Model if needed, otherwise skip | Often required |

---

## Introduction Writing

### The "Intro Formula" (Common in Top 5)

**Paragraph 1**: Motivation + question
- Start with a broad, compelling fact or puzzle
- Narrow to your specific research question
- State the question clearly in 1-2 sentences

**Paragraph 2**: What this paper does
- "In this paper, I/we estimate..."
- State identification strategy clearly
- Explain data sources briefly

**Paragraph 3**: Preview of results
- Main finding with magnitude
- Key robustness results
- Surprising or policy-relevant implications

**Paragraph 4**: Contribution
- "This paper contributes to several strands of literature..."
- Be specific about the marginal contribution
- 2-3 literature strands, 2-3 sentences each

**Paragraph 5 (optional)**: Related literature
- More detailed positioning vs. closest papers
- Explain what you do differently

**Final paragraph**: Roadmap
- "The rest of the paper is organized as follows..."
- Brief, functional

### Common Mistakes in Introductions
- Too long (>5 pages is too long for most papers)
- Burying the research question
- Overselling results
- Vague contribution ("fills a gap in the literature")
- Too much literature review in intro

---

## Table Formatting

### Regression Table Standard

```
Table X: Effect of [Treatment] on [Outcome]
─────────────────────────────────────────────────────
                    (1)        (2)        (3)        (4)
                    OLS        OLS        IV         IV
─────────────────────────────────────────────────────
Treatment          0.123***   0.098**    0.245***   0.201**
                  (0.034)    (0.038)    (0.067)    (0.072)

Control 1                     0.045**               0.039*
                              (0.021)               (0.022)

Control 2                     -0.012                -0.015
                              (0.018)               (0.019)
─────────────────────────────────────────────────────
Unit FE              Yes        Yes        Yes        Yes
Year FE              Yes        Yes        Yes        Yes
Observations       10,000     10,000     10,000     10,000
R²                 0.456      0.461        —          —
First-stage F        —          —        28.4       26.1
─────────────────────────────────────────────────────
Notes: Standard errors clustered at the state level in
parentheses. * p<0.10, ** p<0.05, *** p<0.01. [Additional
notes explaining variables, sample, etc.]
```

### Rules for Tables
1. **Stars**: *, **, *** for 10%, 5%, 1% (economics standard)
2. **Standard errors** in parentheses, directly below coefficient
3. **Never report t-statistics** instead of standard errors
4. **Report N and R²** (or pseudo-R²) for every column
5. **Column headers**: Number + brief description of specification
6. **Fixed effects**: Indicate with "Yes" row
7. **Clustering**: State in table notes
8. **First-stage F**: Report for IV regressions
9. **Dependent variable mean**: Useful for interpreting effect sizes
10. **Use `booktabs` in LaTeX**: `\toprule`, `\midrule`, `\bottomrule`

### Summary Statistics Table

```
Table 1: Summary Statistics
─────────────────────────────────────────────────
Variable          Mean     SD      Min     Max     N
─────────────────────────────────────────────────
Outcome Y        5.234   2.156   0.000   15.670  10,000
Treatment D      0.342   0.474   0.000    1.000  10,000
Control X1       0.512   0.500   0.000    1.000  10,000
Control X2      12.45    3.678   3.000   25.000   9,876
─────────────────────────────────────────────────
Notes: Sample includes [description]. Period: [years].
```

---

## Figure Standards

### Event Study Plot
- X-axis: Time relative to treatment (clearly labeled)
- Y-axis: Coefficient estimate
- Dots: Point estimates
- Bars/shaded: 95% confidence intervals
- Vertical dashed line: Treatment date (t=0)
- Horizontal dashed line: Zero
- Base period (typically t=-1): Marked or noted

### RD Plot
- X-axis: Running variable (centered at cutoff)
- Y-axis: Outcome
- Dots: Binned averages
- Lines: Local polynomial fit (separate above/below cutoff)
- Vertical dashed line: Cutoff

### General Figure Rules
- Clean, minimal design
- Grayscale-friendly (or use colorblind-safe palette)
- Large enough labels to read when printed
- Source noted if using external data
- Number sequentially (Figure 1, Figure 2, ...)

---

## Writing Style

### Economics-Specific Conventions

1. **Active voice preferred**: "We estimate..." not "The effect was estimated..."
2. **First person**: "I" (single author) or "We" (multiple authors or Royal we)
3. **Present tense for results**: "Column 1 shows that..." not "Column 1 showed..."
4. **Interpret magnitudes**: "A one standard deviation increase in X leads to a 5% increase in Y, which corresponds to $2,000 per year"
5. **Statistical vs. economic significance**: Always discuss both
6. **Causal language**: Only use "effect," "impact," "cause" when identification supports it. Otherwise: "associated with," "correlated with," "predicts"
7. **Be concise**: Top economists write clearly and concisely. Avoid jargon when a simpler word works.

### Common Phrases in Economics Papers

| Instead of... | Write... |
|--------------|----------|
| "In order to" | "To" |
| "It is important to note that" | [Delete] |
| "The results of the regression analysis indicate" | "Column 1 shows" |
| "A large body of literature has examined" | "[Author] (Year) and [Author] (Year) study..." |
| "We utilize" | "We use" |
| "We conduct an investigation of" | "We study" |
| "The coefficient is significant at the 5% level" | "The coefficient is 0.12 (SE 0.05)" |

### Reporting Numbers
- **Coefficients**: 2-3 decimal places typically
- **Standard errors**: Same precision as coefficients
- **Percentages**: 1 decimal place
- **Dollar amounts**: Use commas for thousands
- **Large numbers**: 10,000 not 10000
- **Elasticities**: 2 decimal places

---

## Citation Conventions

### Chicago Author-Date (Economics Default)

**In-text:**
- One author: Smith (2020) or (Smith 2020)
- Two authors: Smith and Jones (2020) or (Smith and Jones 2020)
- Three+ authors: Smith et al. (2020) or (Smith et al. 2020)
- Multiple citations: (Smith 2020; Jones 2021)
- Specific page: (Smith 2020, 45)

**Reference list:**
```
Acemoglu, Daron, and James A. Robinson. 2012. Why Nations Fail. New York: Crown.
Angrist, Joshua D., and Jörn-Steffen Pischke. 2009. Mostly Harmless Econometrics. Princeton: Princeton University Press.
Autor, David H., David Dorn, and Gordon H. Hanson. 2013. "The China Syndrome: Local Labor Market Effects of Import Competition in the United States." American Economic Review 103 (6): 2121–68.
```

### JEL Classification Codes (Common)

| Code | Field |
|------|-------|
| C1 | Econometric and Statistical Methods: General |
| C2 | Single Equation Models |
| C3 | Multiple/Simultaneous Equation Models |
| D1 | Household Behavior |
| D4 | Market Structure |
| D8 | Information, Knowledge, and Uncertainty |
| E2 | Macroeconomics: Consumption, Saving, Production |
| E5 | Monetary Policy |
| F1 | Trade |
| G1 | General Financial Markets |
| H2 | Taxation |
| H4 | Publicly Provided Goods |
| I1 | Health |
| I2 | Education |
| J2 | Demand and Supply of Labor |
| J3 | Wages, Compensation |
| J6 | Mobility, Unemployment |
| L1 | Market Structure, Firm Strategy |
| O1 | Economic Development |
| O3 | Innovation; R&D |
| Q5 | Environmental Economics |
| R1 | General Regional Economics |

---

## Replication Package Standards

### AER Data and Code Availability Policy (Since 2005)

Required elements:
1. **README.md**: Clear instructions to reproduce all results
2. **Data**: Raw data (or instructions to obtain restricted data)
3. **Code**: All code to go from raw data to final tables/figures
4. **Mapping**: Which script produces which table/figure
5. **Software requirements**: Version numbers for all software
6. **Runtime estimate**: How long does full replication take?

### Recommended Structure
```
replication/
├── README.md
├── data/
│   ├── raw/           # Raw data files
│   └── processed/     # Cleaned data
├── code/
│   ├── 01_clean.do    # Data cleaning
│   ├── 02_analysis.do # Main analysis
│   ├── 03_robust.do   # Robustness checks
│   └── 04_figures.do  # Figures
├── output/
│   ├── tables/        # Output tables
│   └── figures/       # Output figures
└── paper/
    └── manuscript.tex
```
