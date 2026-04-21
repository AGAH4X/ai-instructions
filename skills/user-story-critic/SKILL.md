---
name: user-story-critic
description: 'Reviews a User Story card and delivers a structured critique: INVEST compliance score, T-shirt size estimate, identified weaknesses, and concrete improvement suggestions. Works interactively — asks for the story if none is provided, then produces a full review.'
---

# User Story Critic

## Goal

Evaluate a User Story card against the INVEST criteria, assign a T-shirt size, surface weaknesses, and provide actionable improvement suggestions so the team can refine the story before committing to a sprint.

Invoke as: `/user-story-critic` (interactive — prompts for the story) or `/user-story-critic "<story text>"` (inline).

---

## Instructions

### Step 1 — Collect the User Story

If a story was passed as an argument, use it directly.

Otherwise, ask the user:

> **"Please paste the User Story you want reviewed (title, description, acceptance criteria, and any other fields you have)."**

Wait for the response before continuing. Do not proceed with partial input — if acceptance criteria are missing, note that in the review rather than asking again.

### Step 2 — Parse the Story

Extract and identify (mark as "not provided" when absent):

- **Title** — the short label
- **As a / I want / So that** — the role, action, and value statement
- **Acceptance Criteria** — the list of conditions that must be met
- **Additional context** — dependencies, mockups, related stories, notes
- **Existing size estimate** — if the team already sized it

### Step 3 — INVEST Analysis

Evaluate each criterion individually. For each, give:

- A **rating**: ✅ Pass / ⚠️ Partial / ❌ Fail
- A **one-sentence justification** grounded in the story text
- A **specific improvement suggestion** when the rating is Partial or Fail

| Criterion | What to check |
|-----------|---------------|
| **I — Independent** | Can this story be developed and delivered without depending on another story being done first? Are there hidden coupling points? |
| **N — Negotiable** | Does the story describe *what* is needed without over-specifying *how*? Is there room for the team to propose solutions? |
| **V — Valuable** | Is the business or user value explicit? Does the "So that" clause name a real benefit? Would a stakeholder care? |
| **E — Estimable** | Does the team have enough information to size this story? Are there unknowns that block estimation? |
| **S — Small** | Can this story be completed within a single sprint (typically 1–5 days of work)? If not, is it a candidate for splitting? |
| **T — Testable** | Are the acceptance criteria specific enough to write automated or manual tests against? Are edge cases covered? |

Compute an **INVEST score**: count the number of ✅ passes out of 6.

### Step 4 — T-Shirt Sizing

Assign a T-shirt size based on the combined assessment of **scope**, **complexity**, **unknowns**, and **dependencies**. Use this rubric:

| Size | Story Points Equivalent | Profile |
|------|------------------------|---------|
| **XS** | 1 | Trivial change. Crystal-clear scope, no unknowns, no dependencies. Can be done in a few hours. |
| **S** | 2–3 | Small, well-understood work. Minimal unknowns. Done in 1–2 days. |
| **M** | 5 | Moderate effort. Some design decisions required. 2–4 days. |
| **L** | 8 | Complex feature. Multiple moving parts or significant unknowns. Likely needs a full sprint. |
| **XL** | 13 | Very large. Should be split before committing to a sprint. Spans multiple areas of the codebase. |
| **XXL** | 21+ | Epic-level. Must be decomposed into smaller stories before it is actionable. |

If the story is missing acceptance criteria or has major unknowns, flag that sizing is unreliable and explain why.

Justify the size in 2–3 sentences referencing specific parts of the story.

### Step 5 — Splitting Suggestions (conditional)

If the size is **L, XL, or XXL**, propose 2–4 concrete ways to split the story into smaller, independently deliverable stories. For each split, briefly explain the value delivered.

### Step 6 — Quality Score

Compute a **global quality score out of 10** by combining three dimensions:

| Dimension | Weight | How to score |
|-----------|--------|-------------|
| INVEST compliance | 5 pts | (INVEST passes / 6) × 5 — partial counts as 0.5 |
| Writing quality | 3 pts | Is the role/action/value clear and specific? Are acceptance criteria precise and unambiguous? Score 0–3. |
| Sizing reliability | 2 pts | Are there enough details to size confidently? No blocking unknowns? Score 0–2. |

Round to one decimal. Add a one-line reading of the score:

- **0–3** — Not ready. Major structural work needed before refinement.
- **4–5** — Weak. Several INVEST criteria fail; AC need a rewrite.
- **6–7** — Acceptable. A few targeted fixes before sprint commitment.
- **8–9** — Good. Minor polish only.
- **10** — Exemplary. Ready to commit as-is.

### Step 7 — Overall Verdict

Summarize the review in 3–5 sentences:

- Is the story ready for sprint commitment?
- What is the single most important thing to fix?
- What is the risk of picking it up as-is?

---

## Output Format

```
## User Story Review

### Parsed Story
- **Title:** ...
- **Role:** As a ...
- **Action:** I want ...
- **Value:** So that ...
- **Acceptance Criteria:** [listed or "not provided"]

---

### INVEST Analysis  (score: N/6)

| # | Criterion | Rating | Notes |
|---|-----------|--------|-------|
| I | Independent | ✅/⚠️/❌ | ... |
| N | Negotiable | ✅/⚠️/❌ | ... |
| V | Valuable | ✅/⚠️/❌ | ... |
| E | Estimable | ✅/⚠️/❌ | ... |
| S | Small | ✅/⚠️/❌ | ... |
| T | Testable | ✅/⚠️/❌ | ... |

#### Improvement Suggestions
- **[Criterion]:** [Concrete rewrite or action to improve this criterion]
- ...

---

### T-Shirt Size: [XS / S / M / L / XL / XXL]

[2–3 sentence justification tied to specific story details]

---

### Splitting Suggestions  *(only if size is L, XL, or XXL)*

1. **[Split story title]** — [What it delivers and why it stands alone]
2. ...

---

### Quality Score: [X.X / 10]  — [one-line reading]

| Dimension | Score |
|-----------|-------|
| INVEST compliance (×5) | X / 5 |
| Writing quality (×3) | X / 3 |
| Sizing reliability (×2) | X / 2 |

---

### Verdict

[3–5 sentence overall assessment: sprint-ready or not, top fix, key risk]
```

---

## Validation Checklist

Before delivering the review:

- [ ] Every INVEST criterion has a rating AND a justification from the story text
- [ ] Improvement suggestions are specific and actionable (not "add more detail")
- [ ] T-shirt size references specific story elements, not just gut feel
- [ ] If size ≥ L, splitting suggestions are present
- [ ] Quality score breakdown is shown (three dimension rows add up to the total)
- [ ] Score reading matches the numeric range
- [ ] Verdict is direct — does not hedge every sentence
- [ ] No section contains unresolved `[placeholder]` text
