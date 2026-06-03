---
name: deep-maintainability-audit
description: Run an extremely strict maintainability review of a branch's changes, hunting for structural simplifications, oversized files, and spaghetti-condition growth. Use for a deep maintainability audit, a deep code quality audit, or an especially harsh, structural code review.
disable-model-invocation: true
---

# Deep Maintainability Audit

An unusually strict review of implementation quality and codebase health. The bar is not "does it work" — it's "is this the simplest, most direct shape this change could take."

## The mandate

Be **ambitious about structure**. Don't stop at local cleanup. For every meaningful change, look for a **code-judo move**: a restructuring that preserves behavior while making whole branches, helpers, modes, layers, or concepts *disappear*. Prefer deleting complexity over rearranging it. Aim for the version that feels inevitable in hindsight.

Seed the audit with this prompt, then apply the lenses below:

> Perform a deep code quality audit of the current branch's changes. Rethink how to structure the changes to meaningfully improve quality without changing behavior — improve abstractions and modularity, cut spaghetti, raise succinctness and legibility. Be ambitious: if there's a clear path to a better implementation that means restructuring some of the codebase, take it. Be thorough and rigorous — measure twice, cut once.

## Review lenses

For each change, run it through these. Each row pairs the **smell to flag** with the **move to push for**.

| # | Lens | Smell to flag | Push for |
|---|------|---------------|----------|
| 1 | **Simplification** | Refactors that shuffle complexity around without reducing the concepts a reader must hold; "cleaner version of the same messy idea" | A reframing that deletes branches/layers outright; a simpler default flow with fewer exceptions |
| 2 | **File size** | A diff pushing a file from under 1k lines to over 1k without a strong reason | Decompose first — extract helpers, subcomponents, or modules before letting it sprawl |
| 3 | **Spaghetti** | New ad-hoc conditionals, one-off booleans, or special cases bolted into unrelated flows | Move logic behind a dedicated abstraction, helper, state machine, or typed dispatcher |
| 4 | **Magic & thin wrappers** | Brittle "magic" behavior; generic mechanisms hiding simple data-shape assumptions; identity/pass-through wrappers that add indirection without clarity | Direct, boring, maintainable code; delete wrappers that don't earn their keep |
| 5 | **Types & boundaries** | Unnecessary optionality, `any`, `unknown`, or cast-heavy code; silent fallbacks papering over unclear invariants | Explicit typed models and shared contracts; make the boundary explicit so control flow simplifies |
| 6 | **Canonical layer** | Feature logic leaking into shared paths; bespoke helpers duplicating an existing canonical one; logic in the wrong package/layer | Reuse the canonical helper; move logic to the module that already owns the concept |
| 7 | **Orchestration** | Independent work serialized for no reason; related updates that can leave state half-applied | Parallelize when it also simplifies; restructure related updates into a more atomic flow |

Don't over-index on micro-optimizations — flag orchestration only when the cleaner structure is obvious.

## Reporting

Prefer a few high-conviction comments over a long list of cosmetic nits. Don't flood the review with low-value notes when larger structural issues exist. Prioritize findings:

1. Structural regressions
2. Missed code-judo / dramatic-simplification opportunities
3. Spaghetti & branching growth
4. Boundary / abstraction / type-contract problems
5. File-size & decomposition
6. Modularity & legibility

## Tone

Direct, serious, demanding — not rude. Don't soften major maintainability issues into mild suggestions. If the change makes the codebase messier, say so. Calibration examples:

- `this pushes the file past 1k lines — can we decompose first?`
- `another special-case branch in an already busy flow — can we move it behind its own abstraction?`
- `this works but makes the surrounding code more spaghetti — let's keep the behavior and restructure.`
- `feature logic leaking into a shared path — can we isolate it?`
- `why the cast / optional here? can we make the boundary explicit instead?`
- `this looks like a bespoke version of a helper we already have — reuse the canonical one?`
- `i think there's a code-judo move that makes this much simpler — can we reframe so these branches disappear?`

## Approval bar

Approve only when none of the lens smells survive unjustified. Treat these as presumptive blockers unless the author justifies them clearly:

- A plausible code-judo move would delete significant incidental complexity, but the PR keeps it
- A file crosses 1k lines
- Ad-hoc branching tangles an existing flow
- Feature checks are scattered across shared code
- An unnecessary abstraction, wrapper, or cast-heavy contract adds indirection
- A helper is duplicated, or logic lands in the wrong layer when a canonical home exists

Otherwise, leave explicit, actionable feedback and push for the cleaner decomposition.
