# Policy Brief Template — Economics Policy Analysis

## Purpose

Template for translating academic economics research into an accessible policy brief. Designed for policymakers, think tanks, and media audiences.

---

## Policy Brief Structure

```markdown
# [Clear, action-oriented title — 10-15 words max]
## [Subtitle with key finding]

**Author(s)**: [Name(s), Affiliation(s)]
**Date**: [Month Year]
**Based on**: [Full paper citation]

---

### Key Takeaways

1. **[Finding 1]**: [1-2 sentences, plain language, with magnitude]
2. **[Finding 2]**: [1-2 sentences]
3. **[Policy implication]**: [1-2 sentences, actionable]

---

### The Problem

[2-3 paragraphs explaining the policy problem]
- Why does this matter? (Scale: how many people affected, how much money)
- What is the current policy landscape?
- What do we not know that would help policymakers?

### What We Did

[2-3 paragraphs, plain language]
- What question did we ask?
- How did we answer it? (Translate identification strategy into accessible language)
  - Avoid jargon: "difference-in-differences" → "compared outcomes before and after the policy change between affected and unaffected groups"
  - Avoid jargon: "instrumental variables" → "used a natural source of variation to isolate the causal effect"
- What data did we use?

### What We Found

[3-4 paragraphs with key results]
- Lead with the most policy-relevant finding
- Use concrete numbers, not regression coefficients
  - ❌ "β = 0.034, p < 0.01"
  - ✅ "The policy increased employment by 3.4 percentage points, equivalent to 50,000 additional jobs"
- Include one key figure (simple, clearly labeled)
- Mention robustness briefly: "This finding holds across multiple specifications and time periods"

### What This Means for Policy

[2-3 paragraphs]
- Direct policy implications
- What should policymakers do (or not do)?
- Important caveats:
  - External validity: Would this work in other contexts?
  - General equilibrium: What happens at scale?
  - Distributional effects: Who gains, who loses?
  - Cost considerations: Is it cost-effective?

### What We Don't Know Yet

[1-2 paragraphs]
- Limitations of this study
- Open questions for future research
- What additional evidence would strengthen these conclusions?

---

**For more information**: [Link to full paper, replication package]
**Contact**: [Email]

**Suggested citation**: [Author(s)] ([Year]). "[Title]." [Policy Brief Series Name], No. [X].
```

---

## Writing Guidelines for Policy Briefs

### Language
- **No jargon**: Replace all technical terms with plain language
- **Active voice**: "The policy increased wages" not "Wages were increased by the policy"
- **Concrete numbers**: Always translate to meaningful units (dollars, people, percentages)
- **Short sentences**: 15-20 words average
- **Short paragraphs**: 3-5 sentences max

### Translation Guide

| Academic | Policy Brief |
|----------|-------------|
| β = 0.05, SE = 0.02 | 5 percentage point increase |
| Statistically significant at the 1% level | Strong evidence that... |
| We exploit exogenous variation from... | We take advantage of a natural experiment where... |
| Difference-in-differences | Comparing before/after the policy, between affected and unaffected groups |
| Instrumental variables | Using an indirect but credible method to isolate the causal effect |
| Regression discontinuity | Comparing individuals just above and just below the eligibility threshold |
| Standard deviation increase | [Translate to dollars/percentage/concrete metric] |
| Elasticity of 0.3 | A 10% increase in [X] leads to a 3% increase in [Y] |
| LATE | The effect for people whose behavior was changed by the policy |
| We fail to reject the null | We do not find evidence that [X] affects [Y] |

### Figure Guidelines
- One key figure only (two maximum)
- Bar chart or line chart preferred
- Large, readable labels
- Clear title that states the finding
- Source noted
- No jargon in axis labels

### Length
- **Executive summary / Key takeaways**: Half a page
- **Full brief**: 4-6 pages total
- **One-pager**: Condensed version with just takeaways + one figure

---

## MVPF Comparison Table Template

When comparing policies, use the Marginal Value of Public Funds framework:

```markdown
### Policy Comparison

| Policy | Causal Effect | WTP | Net Cost | MVPF | Source |
|--------|--------------|-----|----------|------|--------|
| [Policy A] | [Effect in plain language] | $[X] | $[Y] | [Z] | [Citation] |
| [Policy B] | [Effect] | $[X] | $[Y] | [Z] | [Citation] |
| [Policy C] | [Effect] | $[X] | $[Y] | [Z] | [Citation] |

*MVPF = Willingness to Pay / Net Government Cost. Higher MVPF = more cost-effective.*
*MVPF > 1 means the policy passes a cost-benefit test.*
```

---

## Distribution Channels

| Channel | Format | Length | Audience |
|---------|--------|--------|----------|
| NBER Digest | Summary | 1 page | Economists, media |
| VoxEU / VoxDev | Column | 1,500-2,000 words | Economists, policymakers |
| Brookings / AEI / PIIE | Policy brief | 4-6 pages | Policymakers, media |
| The Conversation | Article | 800-1,000 words | General public |
| Twitter/X thread | Thread | 10-15 tweets | Academic community |
| Congressional testimony | Statement | 5-10 pages | Legislators |
