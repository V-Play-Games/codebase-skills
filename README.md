# Codebase Agent Skills

A small collection of reusable skills for coding agents, converted from the original idea notes.

## Included skills

### `codebase-improvement-cycle`
Runs a disciplined, repeatable improvement cycle: orient, diagnose, select one high-leverage issue, design, implement narrowly, verify, update `IMPROVEMENT_LOG.md`, and report.

### `llm-codebase-context`
Guides an agent through inspecting a repository and writing a compact, evidence-based context file for future coding agents.

## Install with the skills CLI

Install all skills from this repository:

```bash
npx skills add YOUR_USERNAME/codebase-skills
```

Install one skill:

```bash
npx skills add YOUR_USERNAME/codebase-skills --skill codebase-improvement-cycle
npx skills add YOUR_USERNAME/codebase-skills --skill llm-codebase-context
```

Replace `YOUR_USERNAME/codebase-skills` with the GitHub owner and repository name after publishing.

## Local testing

From this directory, list the skills recognized by the CLI:

```bash
npx skills add . --list
```

You can also install from a local path:

```bash
npx skills add .
```

## Structure

```text
codebase-skills/
├── README.md
└── skills/
    ├── codebase-improvement-cycle/
    │   └── SKILL.md
    └── llm-codebase-context/
        └── SKILL.md
```

## Notes

- Each skill is a directory containing a `SKILL.md` with YAML frontmatter.
- The `name` in frontmatter matches the directory name.
- The skills preserve the intent of the source notes while making the workflows more explicit and reusable.
- The improvement-cycle skill refers to other skills by name. Those skills need to be available in the agent environment to be consulted; the workflow explains how to proceed if they are not installed.
