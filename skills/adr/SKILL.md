---
name: adr
description: 'Creates a well-structured Architectural Decision Record (ADR) in the current project. Discovers existing ADRs to maintain numbering and format consistency, gathers context from the codebase, and writes the new ADR file in the right location.'
---

# ADR Writer

## Goal

Write a new Architectural Decision Record (ADR) for the current project. The ADR captures the context, options considered, decision made, and its consequences — creating a durable, reviewable record of significant architectural choices.

Invoke as: `/adr` (interactive) or `/adr "Short title of the decision"` (with a title pre-set).

---

## Instructions

### Step 1 — Discover existing ADRs

Search the project for an existing ADR directory and files. Common locations:

- `docs/adr/`, `docs/decisions/`, `docs/architecture/`
- `adr/`, `decisions/`, `architecture/`
- Any `*.md` files with filenames like `0001-*.md` or `ADR-*.md`

From the existing ADRs, extract:

- The storage directory to use for the new ADR
- The numbering scheme (e.g. `0001`, `001`, `1`)
- The next available number (increment the highest existing number by 1)
- The file naming pattern (e.g. `0001-use-postgresql.md`)
- Any project-specific sections or conventions that differ from the default format below

If no ADR directory exists yet, create `docs/adr/` and start numbering from `0001`.

### Step 2 — Establish the decision topic

If a title was passed as an argument, use it directly.

Otherwise, ask the user: **"What architectural decision do you want to record?"**

Wait for their answer before proceeding.

### Step 3 — Gather codebase context

Based on the decision topic, read relevant files to ground the ADR in actual project reality:

- Configuration files, dependency manifests, or infrastructure definitions related to the topic
- Any existing comments, TODOs, or prior discussions in source files
- Related ADRs that this decision builds on or supersedes

Use this context to pre-fill the **Context** and **Consequences** sections with project-specific details rather than generic placeholders.

### Step 4 — Identify options and decision

If not already clear from the context, ask the user:

1. What options were considered?
2. Which option was chosen and why?

Keep the exchange short — one question at a time if needed.

### Step 5 — Write the ADR file

Determine the full file path from the pattern discovered in Step 1 (e.g. `docs/adr/0003-use-event-sourcing.md`).

Write the file using the Output Format below. Use concrete, project-specific content — no generic lorem ipsum placeholders.

Set the status to `Accepted` unless the user indicates otherwise.

---

## Output Format

```markdown
# [NNN]. [Short title of the decision]

Date: YYYY-MM-DD

## Status

Accepted

## Context

[2–4 sentences describing the situation that forced this decision. What problem exists? What constraints apply? Why can't the current approach continue?]

## Decision

[1–3 sentences stating the decision clearly. Start with "We will…" or "We decided to…".]

## Options Considered

### Option 1: [Name]

[Brief description]

**Pros:**
- [Benefit]

**Cons:**
- [Drawback]

### Option 2: [Name]

[Brief description]

**Pros:**
- [Benefit]

**Cons:**
- [Drawback]

## Consequences

### Positive
- [Expected benefit]

### Negative
- [Known trade-off or added complexity]

### Neutral
- [Side-effect that is neither good nor bad]
```

---

## Validation Checklist

Before finalizing the output:

- [ ] ADR number is the correct next increment (no gaps, no duplicates)
- [ ] File is saved in the discovered (or newly created) ADR directory
- [ ] File name matches the project's naming pattern
- [ ] Date is today's date in `YYYY-MM-DD` format
- [ ] Context section explains *why* a decision was needed, not what was decided
- [ ] At least two options are documented, even if one was quickly ruled out
- [ ] Consequences are concrete and specific to this project, not generic
- [ ] No section contains unresolved `[placeholder]` text
