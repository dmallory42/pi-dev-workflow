# pi-dev-workflow

A shareable [pi](https://github.com/badlogic/pi) skill for disciplined software development workflows.

The skill guides agents through a proportional development process:

- discuss and align before coding
- use lightweight specs for small tasks
- use plan + spec for medium/large work
- define observable success criteria
- surface assumptions before they become bugs
- keep scope explicit
- prefer the simplest implementation that satisfies the task
- use TDD and verification where practical
- avoid drive-by changes and over-engineering

## Contents

```text
skills/
  dev-workflow/
    SKILL.md
```

## Install locally

From this checkout:

```bash
pi install /Users/mal/projects/pi-dev-workflow
```

Or from any directory, using the repo path:

```bash
pi install /path/to/pi-dev-workflow
```

## Install from GitHub

After this repo is published to GitHub:

```bash
pi install git:github.com/<user>/pi-dev-workflow
```

You can pin a version/tag:

```bash
pi install git:github.com/<user>/pi-dev-workflow@v0.1.0
```

## Skill

### `dev-workflow`

Use when working on software development tasks, including:

- building features
- fixing bugs
- refactoring code
- creating tools, packages, or libraries
- making plans/specs for code changes

The skill follows a proportional workflow:

- **Small tasks:** discuss → align → spec → build → verify → PR
- **Medium/large tasks:** discuss → align → plan → spec → build → verify → PR

It also includes guardrails for:

1. success criteria
2. assumptions
3. scope
4. simplicity

## Development

The skill source lives at:

```text
skills/dev-workflow/SKILL.md
```

To iterate locally, edit that file and install the package by local path with `pi install`.

## License

MIT
