---
name: grill-me
description: Use when the user wants Codex to stress-test a plan, design, architecture, implementation approach, or code-change strategy before coding. Codex should interrogate assumptions, inspect available files when possible, ask only necessary clarifying questions, and converge on a concrete decision summary.
---

# Grill Me

Use this skill when the user wants a plan, design, architecture, or implementation approach to be challenged before work begins.

This skill is optimized for Codex. Do not rely on interactive popup tools or non-Codex-specific tools. Ask questions in normal chat only when the answer cannot be inferred from the repository, files, command output, or prior context.

## Core behavior

Interrogate the plan until the design is clear enough to implement safely.

Work from the outside in:

1. Identify the goal and success criteria.
2. Identify the affected module or subsystem.
3. Identify the specific methods, files, APIs, data flows, or commands involved.
4. Identify constraints, risks, edge cases, and verification steps.
5. Resolve open decisions before proposing implementation.

Prefer evidence over questions. If a question can be answered by reading code, configuration, tests, docs, logs, or project files, inspect those sources first.

## Question policy

Ask at most one question at a time.

Ask a question only when:

- The answer materially changes the design or implementation.
- The answer cannot be inferred from available files or context.
- Proceeding without it would risk building the wrong thing.

When asking, provide 2–4 concrete options plus an “Other” option. Avoid generic yes/no questions unless the decision is genuinely binary.

Good format:

```text
Which compatibility target should this design preserve?

A. Preserve the current public API exactly
B. Allow internal API changes only
C. Allow public API changes with migration notes
D. Other: ...
```

After the user answers, briefly acknowledge the decision and continue to the next unresolved branch.

## Repository-first workflow

Before asking the user, Codex should usually inspect:

- Relevant `AGENTS.md` files and project instructions.
- Nearby README or design docs.
- Existing module boundaries.
- Current call sites and tests.
- Existing error handling, naming, and style conventions.

Do not ask the user for information that is already discoverable in the codebase.

## Design-tree coverage

Walk the design tree branch by branch. Cover only branches that matter for the current task.

Common branches:

- Goal: What problem is being solved?
- Scope: What is in scope and out of scope?
- Users/callers: Who or what depends on this behavior?
- Inputs/outputs: What data enters and leaves the system?
- State: What persists, mutates, caches, or invalidates?
- Failure modes: What happens on invalid input, timeout, missing data, or partial failure?
- Compatibility: What must remain backward-compatible?
- Security/privacy: What data or permissions are sensitive?
- Performance: What must remain fast or bounded?
- Tests: What proves the design works?
- Rollout: Is migration, feature flagging, or fallback needed?

## Coding constraint

If this skill is used before coding, do not start editing code until the important design branches are resolved.

When implementation is eventually needed:

- Prefer changing one module over multiple modules.
- Prefer changing one method over multiple methods.
- Keep changes local unless the design requires a wider change.
- Preserve existing conventions unless there is a clear reason not to.
- Add or update the smallest relevant verification.

## Completion output

When the grilling phase is complete, provide a concise decision summary:

```text
Decisions resolved:
- Goal: ...
- Scope: ...
- Module/method: ...
- Edge cases: ...
- Verification: ...

Recommended implementation plan:
1. ...
2. ...
3. ...
```

If there are still unresolved decisions, list them explicitly and explain why they block implementation.
