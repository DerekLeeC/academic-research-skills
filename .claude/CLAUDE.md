# Academic Research Skills

A suite of Claude Code skills for rigorous academic research, paper writing, peer review, and pipeline orchestration.

## Skills Overview

| Skill | Purpose | Key Modes |
|-------|---------|-----------|
| `deep-research` v2.2 | Universal 10-agent research team | full, quick, socratic, review, lit-review, fact-check |
| `academic-paper` v2.2 | 10-agent academic paper writing | full, plan, outline-only, revision, abstract-only, lit-review, format-convert, citation-check |
| `academic-paper-reviewer` v1.3 | Multi-perspective paper review (5 reviewers) | full, re-review, quick, methodology-focus, guided |
| `academic-pipeline` v2.2 | Full pipeline orchestrator | (coordinates all above) |
| `economics-research` v1.0 | Economics & management research methodology (8-agent team) | full, identification, methods, data, structural, experimental, policy, macro, socratic |

## Routing Rules

1. **academic-pipeline vs individual skills**: academic-pipeline = full pipeline orchestrator (research → write → review → revise → finalize). If the user only needs a single function (just research, just write, just review), trigger the corresponding skill directly without the pipeline.

2. **deep-research vs academic-paper**: Complementary. deep-research = upstream research engine (investigation + fact-checking), academic-paper = downstream publication engine (paper writing + bilingual abstracts). Recommended flow: deep-research → academic-paper.

3. **deep-research socratic vs full**: socratic = guided Socratic dialogue to help users clarify their research question. full = direct production of research report. When the user's research question is unclear, suggest socratic mode.

4. **academic-paper plan vs full**: plan = chapter-by-chapter guided planning via Socratic dialogue. full = direct paper production. When the user wants to think through their paper structure, suggest plan mode.

5. **academic-paper-reviewer guided vs full**: guided = Socratic review that engages the author in dialogue about issues. full = standard multi-perspective review report. When the user wants to learn from the review, suggest guided mode.

6. **economics-research vs deep-research**: economics-research = domain-specific methodology layer for economics & management (identification strategy, econometric methods, data sources, structural estimation). deep-research = general-purpose research engine. When the research topic is economics/management, trigger economics-research first for methodology design, then hand off to deep-research for literature search and synthesis. economics-research produces an Economics Methodology Brief (EMB) that supplements the RQ Brief.

7. **economics-research mode selection**: `full` = complete methodology design (identification + data + estimation + robustness). `identification` = just causal identification strategy. `methods` = econometric method selection. `data` = dataset recommendations. `structural` = structural model specification. `experimental` = experimental design. `policy` = policy evaluation framework. `macro` = macroeconomic/financial modeling. `socratic` = guided Socratic dialogue on methodology.

## Key Rules

- All claims must have citations
- Evidence hierarchy respected (meta-analyses > RCTs > cohort > case reports > expert opinion)
- Contradictions disclosed with evidence quality comparison
- AI disclosure in all reports
- Default output language matches user input (Traditional Chinese or English)

## Full Academic Pipeline

```
economics-research (if economics/management topic)
  → deep-research (socratic/full)
    → academic-paper (plan/full)
      → academic-paper-reviewer (full/guided)
        → academic-paper (revision)
          → academic-paper-reviewer (re-review, max 2 loops)
            → academic-paper (format-convert → final output)
```

## Handoff Protocol

### economics-research → deep-research
Materials: Economics Methodology Brief (EMB) containing identification strategy, data requirements, estimation approach, robustness plan, and code skeletons. Supplements the RQ Brief with economics-specific technical details.

### economics-research → academic-paper
Materials: EMB informs structure_architect_agent on economics paper structure and draft_writer_agent on disciplinary conventions (table formatting, star notation, JEL codes).

### deep-research → academic-paper
Materials: RQ Brief, Methodology Blueprint, Annotated Bibliography, Synthesis Report, INSIGHT Collection

### academic-paper → academic-paper-reviewer
Materials: Complete paper text. field_analyst_agent auto-detects domain and configures reviewers.

### academic-paper-reviewer → academic-paper (revision)
Materials: Editorial Decision Letter, Revision Roadmap, Per-reviewer detailed comments

## Version Info
- **Version**: 2.0
- **Last Updated**: 2025-03-05
- **Author**: Cheng-I Wu
- **License**: CC-BY-NC 4.0
