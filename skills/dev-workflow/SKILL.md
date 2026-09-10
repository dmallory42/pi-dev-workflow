---
name: dev-workflow
description: Preferred development workflow for software tasks. Covers discussion, planning, spec writing, TDD-driven build, verification, and PR creation. Use when the user asks for help with any software development task.
license: MIT
---

# Dev Workflow

Follow this workflow for all software development tasks.

## When to Use

When the user asks to:

- build something new
- fix a bug
- add a feature
- refactor code
- create a tool, package, or library
- work on any software development task

## Workflow Steps

Use the workflow at a weight appropriate to the task size:

- **Small tasks**: discuss → align → spec → build → verify → PR. A separate `plan.md` is optional and usually unnecessary.
- **Medium/large tasks**: discuss → align → plan → spec → build → verify → PR.

Infer the likely task size from signals in the user's request and the issue context. For example, wording like "quick fix", "one-liner", "small typo", "rename", "update this text", or a narrowly scoped bug usually indicates a small task. Broad feature requests, ambiguous behavior changes, multiple affected systems, data migrations, concurrency/state changes, or unclear acceptance criteria usually indicate medium/large work.

If the task appears quick or one-line, check with the user before using the lightweight path, e.g. "This looks like a small task; I'll skip the separate plan and write a compact spec unless you want the full workflow." If uncertain, start lightweight and add a plan only when the trade-offs, scope, or sequencing need it.

## Coding Guardrails

Before writing code, satisfy these gates at a level proportional to the task size. For small tasks, this can be a short sentence or two. For larger tasks, capture the answers in the plan/spec.

1. **Success Criteria** — State what observable outcome means the task is done, e.g. "Success means test X passes" or "Success means the README text is updated and the build still passes."
2. **Assumptions** — Surface important assumptions about user intent, existing behavior, requirements, or constraints. Verify them in code where possible; ask when they are user preferences or materially affect the solution.
3. **Scope** — Define the boundary of the work: files, functions, systems, and what is explicitly off-limits. Avoid drive-by fixes and "while I'm here" changes.
4. **Simplicity** — Start with the simplest approach that satisfies the spec. Add abstractions, configuration, dependencies, or generalized APIs only when current requirements justify them.

For a likely small task, a lightweight guardrail check is enough, for example:

> This looks small. Success means the typo is fixed and existing checks still pass. I'm assuming no behavior change is intended. I'll only touch `README.md`. Simplest approach: direct text edit.

Red flags that require stopping to clarify or re-scope:

- You cannot state observable success criteria
- You are thinking "they probably want..." about an important behavior or preference
- The solution is expanding beyond the agreed files/systems
- You are adding abstractions, configuration, helpers, or dependencies for hypothetical future use
- You are about to modify unrelated code or perform drive-by cleanup

### 1. Discuss

- Understand the goal by asking clarifying questions
- Talk through the approach and options
- Do not start coding or planning until the goal is clear

### 2. Align

- Confirm scope, naming, structure, and tech choices
- State success criteria: what observable outcome means the task is done
- Surface important assumptions and resolve uncertain or preference-dependent ones
- Identify what is explicitly out of scope
- Confirm the simplest reasonable approach for the current requirements
- Get the user's agreement before proceeding

### 3. Plan

- For small tasks, this step may be skipped after detecting the task is likely lightweight and explicitly confirming with the user
- For medium/large tasks, write a planning document at `~/.pi/docs/specs/<task-name>/plan.md`
- The plan captures high-level thinking, options, trade-offs, and open questions
- Use this format:

```markdown
# Plan: <task name>

- Created: YYYY-MM-DD
- Updated: YYYY-MM-DD

## Goal

One-sentence summary of what we are trying to achieve.

## Context

Why this work is needed. Background and motivation.

## Approach

High-level approach, options considered, and trade-offs.

## Open Questions

Anything unresolved that needs input before specifying.
```

- Present the plan to the user and wait for approval before moving to spec
- If the plan step was skipped for a small task, proceed directly to spec after alignment

### 4. Spec

- Turn the approved plan, or the aligned scope for small tasks, into a formal specification at `~/.pi/docs/specs/<task-name>/spec.md`
- Use this format:

```markdown
# Spec: <task name>

- Created: YYYY-MM-DD
- Updated: YYYY-MM-DD

## Goal

One-sentence summary.

## Background

Context and motivation.

## Requirements

1. Requirement one
2. Requirement two
3. ...

## Acceptance Criteria

These should be observable and verifiable; they are the concrete success criteria for the task.

- [ ] Criterion derived from requirement 1
- [ ] Criterion derived from requirement 2
- [ ] ...

## Out of Scope

- Item one
- Item two

## Subtasks

If the work is large, break it into discrete subtasks. Each subtask should have its own requirements and acceptance criteria.

### Subtask 1: <name>

**Requirements:**

1. ...

**Acceptance criteria:**

- [ ] ...
```

- Present the spec to the user and wait for approval before building
- The spec is the contract: tests and implementation are measured against it

### 5. Build

- Create a feature branch
  - Follow the repo's branch naming conventions
  - If none exist, use descriptive branch names like `feat/task-name` or `fix/task-name`
- Establish or confirm the baseline before adding task-specific tests
  - If the repo/test environment is already broken, briefly document the issue
  - Fix or isolate pre-existing breakage before treating new tests as the task signal
  - If baseline repair is significant, confirm with the user before expanding scope
- Write failing tests first, derived from the spec's acceptance criteria
  - Use whatever test framework the project already uses
  - Show the failing tests to the user before implementing when practical
  - If setup or baseline repair must happen first, explain that before proceeding
- Implement until tests pass
- Stay inside the agreed scope; do not make drive-by fixes or unrelated cleanups
- Do not remove or rewrite existing comments unless required by the change. If a comment appears stale or misleading but is outside scope, mention it separately instead of changing it.
- Prefer the simplest implementation that satisfies the spec; avoid single-use abstractions unless they clearly improve readability for the current change
- Work incrementally: do not write large amounts of code before running tests
- Between meaningful subtasks, give a brief checkpoint recap:
  - What is done
  - What is next
  - Whether the spec still looks right or needs adjustment
- Keep commits small with brief, descriptive commit messages

### 6. Verify

- Run the appropriate level of testing for the task:
  - Unit tests
  - Integration or end-to-end tests
  - Smoke tests
- Run typecheck if applicable
- Ensure all tests pass
- Do not move to PR until verification is complete

### 7. PR

- Create a pull request via `gh pr create` unless told otherwise
  - Some repos may define alternative PR creation paths; follow those if specified
- PR description format (use repo conventions if available, otherwise default to):

```markdown
## What

Brief description of what this PR does.

## Why

Motivation and link to the spec if applicable.

## How to Test

Steps to verify the changes work.
```

## What This Workflow Does NOT Cover

- Publishing or releasing
- CI setup
- Post-merge activities

These are handled separately if and when the user asks.

## Guidelines

- Do not skip steps silently. If the user wants to jump ahead, confirm which steps to skip explicitly.
- Use signals from the request and repo context to detect likely quick/one-liner tasks, then confirm the lightweight path with the user.
- For small tasks, skipping the separate plan document is allowed after alignment; the spec remains the contract.
- Do not start coding before the spec is approved.
- Do not push directly to main. Always use a branch and PR.
- Do not add features the user did not ask for. Suggest extras as options, not defaults.
- Avoid "while I'm here" changes. If you notice adjacent cleanup or unrelated bugs, mention them separately and ask before acting.
- Keep the dependency footprint small. Prefer existing tools on the machine where reasonable.
- Be upfront about limitations and what the current version will not do.
- Frame out-of-scope items clearly in the spec so they are not forgotten.
