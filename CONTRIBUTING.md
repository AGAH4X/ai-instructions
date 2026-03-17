# Contributing

Thank you for wanting to contribute to this repo! Here's how to get started.

## Adding an instruction, skill, or agent

### 1. Create your branch

Name your branch using the following convention:

```bash
git checkout -b feature/my-instruction
```

Valid examples:
- `feature/pr-reviewer-skill`
- `feature/github-copilot-agent`
- `feature/commit-message-instructions`

### 2. Add your content

Place your files in the appropriate folder based on the contribution type:

```
skills/        → Claude Code skills (SKILL.md files)
agents/        → AI agents (.agent.md files)
instructions/  → System instructions or reusable prompts
```

If the folder doesn't exist yet, create it with a descriptive name.

### 3. Open a Pull Request

Push your branch and open a PR targeting `master`:

```bash
git push origin feature/my-instruction
```

In the PR description, explain:
- What your instruction / skill / agent does
- When and how to use it

All contributions are reviewed before being merged.
