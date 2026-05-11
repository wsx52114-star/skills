---
trigger: always_on
glob:
description: Antigravity Dynamic Skill Index
---

# Antigravity Agent Skills Index

You have access to several behavioral skills. **Whenever the user's prompt matches the trigger description for a skill, you MUST use the `view_file` tool to read the corresponding SKILL.md file BEFORE taking any other action.** 
Do NOT guess the workflow. Always read the file first to get the detailed steps.

**Important**: 
- **Domain Language & Framework Docs**: Both project domain language and agent framework documentation are unified in `.agents/CONTEXT.md`.
- **Architecture Decisions (ADR)**: All generated ADRs should be written to `.agents/docs/adr/`. Do NOT use the root directory.

## Golden Workflow for New Features

When the user requests a **new feature** (not a bug fix or minor tweak), you MUST NOT jump straight into writing code or a flat implementation plan. Instead, orchestrate the following phased skill execution:

### Phase 1 — Architecture Grilling (REQUIRED, then STOP)
1. Invoke the `grill-with-docs` skill (read `.agents/skills/engineering/grill-with-docs/SKILL.md` first).
2. Run the grilling session: challenge the plan against `.agents/CONTEXT.md`, sharpen terminology, and update the file and ADRs in `.agents/docs/adr/` as decisions crystallise.
3. **STOP. Request user approval before proceeding.** Do NOT continue to Phase 2 until the user explicitly confirms.

### Phase 2 — Issue Breakdown (after user approval)
4. Invoke the `to-issues` skill (read `.agents/skills/engineering/to-issues/SKILL.md` first).
5. Break the confirmed plan into independently executable issues (vertical slices). Present the issue list to the user.

### Phase 3 — Implementation (per issue)
6. For each issue, invoke the `tdd` skill (read `.agents/skills/engineering/tdd/SKILL.md` first).
7. Follow strict red-green-refactor: write failing test → implement → pass → refactor.

### Phase 4 — Diagnosis (on-demand)
8. If any unexpected error or regression arises at any point in Phase 3, **immediately** invoke the `diagnose` skill (read `.agents/skills/engineering/diagnose/SKILL.md` first) before making any guesses or blind fixes.

> This phased workflow applies ONLY to new feature development. For bug fixes, minor tweaks, or refactors, proceed directly with the appropriate skill without this orchestration.

## Available Skills:

- **tdd**: Test-driven development with red-green-refactor loop. Use when user wants to build features or fix bugs using TDD, mentions "red-green-refactor", wants integration tests, or asks for test-first development.
  - **Path**: `.agents/skills/engineering/tdd/SKILL.md`

- **improve-codebase-architecture**: Find deepening opportunities in a codebase, informed by the domain language in `.agents/CONTEXT.md` and the decisions in `.agents/docs/adr/`. Use when the user wants to improve architecture, find refactoring opportunities, consolidate tightly-coupled modules, or make a codebase more testable and AI-navigable.
  - **Path**: `.agents/skills/engineering/improve-codebase-architecture/SKILL.md`

- **zoom-out**: Tell the agent to zoom out and give broader context or a higher-level perspective. Use when you're unfamiliar with a section of code or need to understand how it fits into the bigger picture.
  - **Path**: `.agents/skills/engineering/zoom-out/SKILL.md`

- **to-issues**: Break a plan, spec, or PRD into independently-grabbable issues on the project issue tracker using tracer-bullet vertical slices. Use when user wants to convert a plan into issues, create implementation tickets, or break down work into issues.
  - **Path**: `.agents/skills/engineering/to-issues/SKILL.md`

- **setup-matt-pocock-skills**: Sets up an `## Agent skills` block in AGENTS.md/CLAUDE.md and `docs/agents/` so the engineering skills know this repo's issue tracker (GitHub or local markdown), triage label vocabulary, and domain doc layout. Run before first use of `to-issues`, `to-prd`, `triage`, `diagnose`, `tdd`, `improve-codebase-architecture`, or `zoom-out` — or if those skills appear to be missing context about the issue tracker, triage labels, or domain docs.
  - **Path**: `.agents/skills/engineering/setup-matt-pocock-skills/SKILL.md`

- **to-prd**: Turn the current conversation context into a PRD and publish it to the project issue tracker. Use when user wants to create a PRD from the current context.
  - **Path**: `.agents/skills/engineering/to-prd/SKILL.md`

- **grill-with-docs**: Grilling session that challenges your plan against the existing domain model, sharpens terminology, and updates documentation (`.agents/CONTEXT.md`, ADRs in `.agents/docs/adr/`) inline as decisions crystallise. Use when user wants to stress-test a plan against their project's language and documented decisions.
  - **Path**: `.agents/skills/engineering/grill-with-docs/SKILL.md`

- **diagnose**: Disciplined diagnosis loop for hard bugs and performance regressions. Reproduce → minimise → hypothesise → instrument → fix → regression-test. Use when user says "diagnose this" / "debug this", reports a bug, says something is broken/throwing/failing, or describes a performance regression.
  - **Path**: `.agents/skills/engineering/diagnose/SKILL.md`

- **triage**: Triage issues through a state machine driven by triage roles. Use when user wants to create an issue, triage issues, review incoming bugs or feature requests, prepare issues for an AFK agent, or manage issue workflow.
  - **Path**: `.agents/skills/engineering/triage/SKILL.md`

- **grill-me**: Interview the user relentlessly about a plan or design until reaching shared understanding, resolving each branch of the decision tree. Use when user wants to stress-test a plan, get grilled on their design, or mentions "grill me".
  - **Path**: `.agents/skills/productivity/grill-me/SKILL.md`

- **write-a-skill**: Create new agent skills with proper structure, progressive disclosure, and bundled resources. Use when user wants to create, write, or build a new skill.
  - **Path**: `.agents/skills/productivity/write-a-skill/SKILL.md`

- **caveman**: >
  - **Path**: `.agents/skills/productivity/caveman/SKILL.md`

- **git-guardrails-claude-code**: Set up Claude Code hooks to block dangerous git commands (push, reset --hard, clean, branch -D, etc.) before they execute. Use when user wants to prevent destructive git operations, add git safety hooks, or block git push/reset in Claude Code.
  - **Path**: `.agents/skills/misc/git-guardrails-claude-code/SKILL.md`

- **scaffold-exercises**: Create exercise directory structures with sections, problems, solutions, and explainers that pass linting. Use when user wants to scaffold exercises, create exercise stubs, or set up a new course section.
  - **Path**: `.agents/skills/misc/scaffold-exercises/SKILL.md`

- **migrate-to-shoehorn**: Migrate test files from `as` type assertions to @total-typescript/shoehorn. Use when user mentions shoehorn, wants to replace `as` in tests, or needs partial test data.
  - **Path**: `.agents/skills/misc/migrate-to-shoehorn/SKILL.md`

- **setup-pre-commit**: Set up Husky pre-commit hooks with lint-staged (Prettier), type checking, and tests in the current repo. Use when user wants to add pre-commit hooks, set up Husky, configure lint-staged, or add commit-time formatting/typechecking/testing.
  - **Path**: `.agents/skills/misc/setup-pre-commit/SKILL.md`

