---
name: codebase-expert-generator
description: 'Scans any project (any language or stack) and generates a structured codebase-expert.agent.md file that captures architecture, tech stack, conventions, workflows, and business context — ready to be used as a context provider for other AI agents in orchestration scenarios.'
---

# Codebase Expert Generator

## Goal

Analyze the current project and produce a `codebase-expert.agent.md` file that can act as a knowledgeable context agent for any downstream AI agent (reviewer, debugger, onboarding assistant, automation agent, etc.).

This skill works on any project — monorepo, microservices, frontend, backend, CLI tool, library — regardless of language or stack.

---

## Instructions

### Step 1 — Scan the project structure

Use file system tools to explore the project root. Collect:

- Full directory tree (at least 3 levels deep, excluding `node_modules`, `.git`, build artifacts)
- Key config files: package.json, pyproject.toml, go.mod, Cargo.toml, pom.xml, build.gradle, .env.example, docker-compose.yml, Makefile, etc.
- Entry points: main files, index files, app bootstraps
- Test directories and test file patterns
- CI/CD config files (.github/workflows, .gitlab-ci.yml, Jenkinsfile, etc.)
- Documentation files (README, docs/, CHANGELOG, ADRs)
- Linting / formatting config (.eslintrc, .prettierrc, ruff.toml, etc.)

### Step 2 — Identify the tech stack

From the files collected above, extract:

- Programming languages and their versions
- Frameworks and major libraries (with versions if available)
- Build tools, bundlers, task runners
- Databases and ORMs
- Testing frameworks
- Deployment targets (Docker, serverless, Kubernetes, etc.)

### Step 3 — Extract conventions and best practices

Read the following if they exist:

- README and contributing guides
- Linting and formatting rules
- Commit message conventions (commitlint, conventional commits, etc.)
- Code review checklists
- Any CLAUDE.md or .cursorrules files

Summarize the patterns, naming conventions, folder organization, and coding standards actually used in the project.

### Step 4 — Identify business context and workflows

Read README, docs, and source files to extract:

- The domain and purpose of the application
- Primary user-facing features and flows
- Key API routes or entry points
- CI/CD pipeline steps (test, build, deploy)
- Data models or schema if accessible

### Step 5 — Identify common pitfalls

Based on the stack and codebase patterns, list:

- Known gotchas specific to this stack or project
- Areas of the code that are fragile or frequently changed
- Any explicit warnings found in comments or docs

### Step 6 — Generate the output file

Write the file `codebase-expert.agent.md` in the project root using the structure defined below. The file must be written in a way that an AI agent can load it as context and immediately act as a codebase expert without additional exploration.

---

## Output Format: `codebase-expert.agent.md`

```markdown
---
name: codebase-expert
description: 'Expert agent for the [Project Name] codebase. Provides authoritative context on architecture, stack, conventions, and workflows for use in agent orchestration.'
tools: Read, Glob, Grep
---

# [Project Name] — Codebase Expert

## Role

You are an expert on the [Project Name] codebase. You have deep knowledge of its architecture, tech stack, conventions, and business logic. Your role is to provide accurate, actionable context to other AI agents working on this project.

When answering questions, always ground your response in the actual code — use your Read, Glob, and Grep tools to verify before answering.

---

## Project Structure

[Paste the directory tree here — at least 3 levels deep]

Key directories:
- `src/` — [purpose]
- `tests/` — [purpose]
- `docs/` — [purpose]
- [other notable directories]

---

## Tech Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Language | ... | ... |
| Framework | ... | ... |
| Database | ... | ... |
| Testing | ... | ... |
| Build | ... | ... |
| CI/CD | ... | ... |

---

## Business Context

**Domain:** [Brief description of what this application does and for whom]

**Key features:**
- [Feature 1]
- [Feature 2]
- [Feature 3]

**Entry points:**
- [Main file or route file paths]

---

## Conventions & Best Practices

**Code style:**
- [Formatting rules, linter config summary]
- [Naming conventions — files, functions, variables, classes]

**Project patterns:**
- [Folder structure conventions]
- [How modules are organized]
- [Import style]

**Testing:**
- [Test file naming pattern]
- [Test structure conventions]
- [How to run tests]

**Git:**
- [Branch naming convention]
- [Commit message format]
- [PR process]

---

## Key Workflows

### Development
[How to start the dev server, run tests, build]

### CI/CD
[Pipeline steps — test → build → deploy]

### [Other domain-specific workflow, e.g. "Authentication Flow", "Data Ingestion Pipeline"]
[Description of the flow with relevant file paths]

---

## Common Pitfalls

- [Gotcha 1 — e.g. "Do not mutate X directly, use the Y helper"]
- [Gotcha 2 — e.g. "Migration files must be run manually after pulling"]
- [Gotcha 3 — e.g. "Tests require a running local database"]

---

## Orchestration Guidance

This agent is designed to be loaded as context by other agents. Example use cases:

- **Code review agent**: Load this agent to understand conventions before reviewing a PR
- **Debugging agent**: Load this agent to know where to look when tracing a bug
- **Onboarding agent**: Load this agent to answer "where is X?" or "how does Y work?" questions
- **Feature planning agent**: Load this agent to understand constraints before proposing a design

When orchestrating, pass this file as the system context or as an initial tool call result.
```

---

## Validation Checklist

Before finalizing the output:

- [ ] Directory tree is complete and accurate
- [ ] Tech stack table has no unknown entries — verify against actual config files
- [ ] Conventions section reflects actual code, not guesses
- [ ] At least one end-to-end workflow is documented
- [ ] Common pitfalls section is non-empty
- [ ] The file can be read cold by another agent and provide immediate value
