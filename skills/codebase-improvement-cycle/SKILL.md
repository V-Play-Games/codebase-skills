---
name: codebase-improvement-cycle
description: "Run one safe, evidence-based codebase improvement cycle: inspect the repository, identify the highest-leverage design weakness, make one narrowly scoped change, verify it with real checks, and record the result in IMPROVEMENT_LOG.md. Use when asked to incrementally improve, refactor, or maintain an existing codebase across repeatable cycles."
---

# Codebase Improvement Cycle

## Mission

This is a standing process, not a one-time cleanup or rewrite. Each cycle finds the single highest-leverage design weakness, fixes it safely, verifies the change, and records what happened so another agent or session can continue without re-deriving the reasoning.

A good cycle is small, boring, and fully verified. Avoid sprawling diffs. Ten good cycles are better than one heroic rewrite.

## Operating principles

- Complete exactly one backlog item per cycle.
- Prefer small, reviewable changes with low blast radius.
- Never combine a behavior change with a structural refactor in one edit.
- Never mark a cycle complete without actually running the relevant tests and checks.
- Never silently change public behavior. Record the change and ask for human review before implementing it.
- Do not add a pattern, defensive check, abstraction, or indirection unless it resolves a diagnosed problem.
- If tests cannot be made to pass or the right fix requires a product decision, stop and escalate instead of guessing.
- Do not push, merge, or open a pull request unless explicitly asked.

## Skill library

Consult the following skills by name at the phase where they are needed. Do not load all of them at the beginning; most cycles only need a subset.

| Phase | Skill | Purpose |
|---|---|---|
| Orient | `code-standards` | Learn this codebase's actual conventions before suggesting changes |
| Diagnose | `aposd-reviewing-module-design` | Find shallow interfaces, information leakage, and needless complexity |
| Diagnose | `cc-control-flow-quality` | Find deep nesting, high cyclomatic complexity, and missing guard clauses |
| Diagnose | `ca-architecture-boundaries` | Find boundary violations and wrong-direction dependencies |
| Diagnose | `code-clarity-and-docs` | Find naming, comment, and documentation gaps |
| Design | `aposd-simplifying-complexity` | Decide where complexity should live; pull it downward, not outward |
| Design | `cc-pseudocode-programming` | Design the routine in pseudocode before writing real code |
| Design | `gof-design-patterns` | Apply a pattern only when its forces genuinely match the symptom |
| Design | `aposd-designing-deep-modules` | Check that the resulting interface is deep, not shallow |
| Design | `cc-routine-and-class-design` | Check cohesion, parameter counts, and single responsibility |
| Implement | `cc-refactoring-guidance` | Fix-first-then-refactor discipline and safe refactoring workflow |
| Implement | `cc-defensive-programming` | Boundary validation, assertions versus exceptions |
| Verify | `aposd-verifying-correctness` | Check functional correctness, error states, and security |
| Verify | `cc-quality-practices` | Judge whether tests cover the change at a level appropriate to risk |

If a named skill is unavailable, continue using the process below and do not pretend the skill was consulted.

## Cycle workflow

### 1. Orient

1. Read `IMPROVEMENT_LOG.md` if it exists. Resume the existing process rather than starting over.
2. Consult `code-standards` to confirm the repository's real naming, module layout, testing structure, and error-handling conventions.
3. On cycle 1, establish those conventions from the codebase. Match the existing project rather than imposing textbook examples.

If `IMPROVEMENT_LOG.md` does not exist, create it with the empty sections shown below before proceeding.

### 2. Diagnose without editing

On cycle 1, conduct a broad pass weighted toward modules that many other parts of the code depend on. On later cycles, inspect what has changed since the last cycle plus one new area; do not re-audit everything every time.

Consult these skills as relevant:
- `aposd-reviewing-module-design`
- `cc-control-flow-quality`
- `ca-architecture-boundaries`
- `code-clarity-and-docs`

For each discovered weakness, append a backlog row describing its location, what is wrong, the principle it violates, and its current severity. Do not fix anything during diagnosis, even if the issue looks obvious.

Severity should reflect pain caused today, not aesthetic preference.

### 3. Select exactly one target

Choose one existing backlog item. Prioritize, roughly in this order:

1. A weakness that many callers already work around; shallow or leaky modules often have more leverage than an ugly leaf function.
2. High-churn code over stable code that is rarely touched.
3. The smallest change that meaningfully closes the gap over a theoretically complete redesign.
4. Low-blast-radius fixes early in the process; save sweeping boundary work until there is a trustworthy test safety net.

If the target has little or no test coverage, first write characterization tests for current behavior. Do not restructure behavior that cannot be verified.

Record the chosen item, why it was selected, and what was passed over.

### 4. Design before implementation

1. Consult `cc-pseudocode-programming` and sketch the routine in pseudocode before writing real code. Iterate on the pseudocode, not the implementation.
2. For structural or shallow-interface problems, consult `aposd-designing-deep-modules` and `aposd-simplifying-complexity`. The interface should hide more than it exposes, and complexity should move down into the module rather than spread across callers.
3. Consult `cc-routine-and-class-design` for cohesion, parameter count, and single responsibility.
4. Use `gof-design-patterns` only when a specific pattern's forces match the diagnosed symptom. Do not add indirection for its own sake.
5. If the fix changes a public interface or externally visible behavior, document that explicitly and flag it for human review before implementing it.

### 5. Implement narrowly

1. Consult `cc-refactoring-guidance`.
2. If behavior is wrong, fix the behavior first in the smallest safe change, verify it, and only then restructure in a separate edit or cycle. Never mix a behavior change and a structural refactor in one edit.
3. Consult `cc-defensive-programming` for any boundary touched. Validate at the edge; choose deliberately between assertions and runtime exceptions; avoid scattering redundant checks through internal code.
4. Keep the diff focused on the selected target.
5. If another issue is discovered, add it to the backlog and leave it untouched.

### 6. Verify with actual commands

1. Consult `aposd-verifying-correctness` to check normal paths, errors, edge cases, and security-relevant boundaries.
2. Consult `cc-quality-practices` to decide whether existing tests cover the change. Add tests proportional to risk, not padding.
3. Actually run the project's tests and relevant linters or type-checkers.
4. If any check fails, fix the failure or revert the change. Do not mark a red cycle as complete.
5. Record the exact commands and their results. Never claim verification that was not performed.

### 7. Update documentation and the log

Consult `code-clarity-and-docs` for comments, docstrings, READMEs, or architecture notes affected by the change. If this cycle establishes or changes a convention, update the source used to document `code-standards`.

Maintain `IMPROVEMENT_LOG.md` at the repository root. It is the process's persistent memory between cycles.

Use this structure:

```markdown
# Improvement Log

## Backlog
| Found | Location | Weakness | Principle | Severity | Status |
|---|---|---|---|---|---|

## Cycle history
```

For each completed cycle, add a dated history entry containing:
- Target
- Why it was chosen over other candidates
- Skills applied
- Change made
- Verification commands and results
- Deferred issues, added as backlog rows
- Natural next target

Move the completed backlog item into cycle history or otherwise mark it clearly as completed; do not lose its history.

### 8. Commit and report

If the repository uses Git, create one commit for the cycle with a message derived from the log entry. Do not push, merge, or open a PR unless asked.

Report in a few sentences:
- What changed
- Why it was the highest-leverage choice available
- What verification actually ran and its result
- What remains deferred and the likely next target

Then stop unless explicitly asked to run in batch mode.

## Backlog example

```markdown
| Found | Location | Weakness | Principle | Severity | Status |
|---|---|---|---|---|---|
| 2026-08-23 | src/billing/Invoice.ts | applyDiscount has 6 nested conditionals | control flow | high | open |
```

## Cycle history example

```markdown
### 2026-08-23 — Cycle 14
- **Target:** Invoice.applyDiscount nesting
- **Why this one over others:** touched in ~40% of recent commits; every caller re-implements its own guard logic
- **Skills applied:** cc-control-flow-quality, cc-pseudocode-programming, cc-refactoring-guidance
- **Change:** extracted guard clauses, replaced nesting with early returns, no behavior change
- **Verification:** 38 existing tests green; added 4 for zero-quantity and expired-coupon edges
- **Docs touched:** docstring on applyDiscount
- **Deferred (new backlog rows):** discount-rule lookup leaks a raw Map across the module boundary
- **Next up:** that leak, or the OrderController boundary violation — roughly equal leverage
```

Treat examples as formatting examples, not as facts about the current repository. Use the current date and actual repository evidence for real entries.
