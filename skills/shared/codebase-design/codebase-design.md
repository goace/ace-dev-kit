# Codebase Design

## When to use

Use for module design, API changes, decomposition, refactoring, naming, comments, tests, performance work, and changes that feel awkward or spread complexity across files.

Apply relevant rules within the task's scope and confirmed constraints. This is a design reference, not an extra workflow; mechanical changes need no separate design review.

## Primary bias to correct

Working code, small pieces, familiar patterns, flags, wrappers, and extra documentation do not make a design simple when they increase cognitive load or leak knowledge. Treat red flags as investigation prompts, not verdicts; compare total complexity for callers and maintainers while preserving required behavior.

## Decision rules

- A **module** has an **interface** and an **implementation**. The interface includes everything callers must know: behavior, inputs, results, side effects, invariants, ordering, lifecycle, errors, configuration, and relevant performance guarantees. The implementation fulfills that contract; a signature expresses only part of it.
- Use reduced complexity as the primary success metric. Prefer the design that lowers cognitive load, change amplification, hidden dependencies, temporal coupling, and the number of facts a reader must hold at once.
- Treat design as continuous work. A first working patch is not done if it worsens future changeability; compare plausible alternatives for non-trivial interface, decomposition, or abstraction choices.
- Prefer deep modules: small, semantic interfaces that hide meaningful internal complexity. Reject pass-through services, thin library wrappers, helper modules, and tiny split-outs that add names without reducing reader burden.
- **Depth** provides **leverage**: useful behavior for less caller knowledge. It also supports **locality**: related changes and fixes stay together. Judge these benefits, not implementation length, method counts, or parameter counts.
- Design interfaces around what callers need to know, not how the implementation works. Avoid fragile staging, setup sequences, mode flags, configuration knobs, and arguments that expose internal choices.
- Hide volatile decisions, internal representations, storage shape, protocols, file formats, performance hacks, bookkeeping, normalization, and messy edge handling inside the module that owns the knowledge.
- Pull complexity downward when the lower module owns the detail. Prefer a slightly more complex implementation if it gives callers a simpler public contract and removes repeated reasoning from call sites.
- Choose generality at the right level. Avoid one-caller overfitting, vague speculative abstractions, and core paths polluted by rare edge cases; isolate special behavior with special-general decomposition.
- Combine or split by total complexity, not by size, runtime order, habit, or aesthetics. Keep related state, behavior, invariants, and design decisions together unless the new boundary is deeper and independently understandable.
- Reduce exception surface by changing interfaces or invariants where possible. Define away invalid states and awkward cases instead of making every caller repeat defensive ceremony. Preserve required business distinctions: catching failures, returning defaults, or adding validation does not itself eliminate errors.
- Use comments to reduce complexity: document interface contracts, invariants, hidden design decisions, rationale, and tricky implementation facts callers should not need to know. Do not narrate code or compensate for bad names, poor decomposition, or confusing flow.
- Treat names, consistency, and obviousness as design information. Names should reveal abstractions rather than mechanisms; related operations should share conventions; surprising code is complexity even when short.
- Use tests to protect behavior through public contracts and stable APIs, especially around hidden complexity and isolated special cases. Do not let test convenience force shallow or leaky interfaces. Internal modules can have their own interfaces and tests; keep internal seams private.
- A **seam** allows behavior substitution without changing its caller; an **adapter** satisfies the interface there. Introduce substitution for actual variation, useful test isolation, or a stable integration contract. Adapter counts alone neither justify nor invalidate a seam.
- Accept dependencies where composition belongs when substitution is useful; keep internal assembly out of ordinary callers' interfaces. Prefer returned values for pure calculations; make required side effects explicit.
- Add performance optimizations, trends, paradigms, patterns, or frameworks only when they reduce complexity in this codebase or evidence shows the tradeoff matters; hide optimization details behind stable interfaces.

## Trigger rules

- When a feature feels awkward, one change spreads across files, or reviewers must reconstruct hidden dependencies, look for missing information hiding, shallow modules, temporal coupling, or complexity pushed to callers.
- When adding a module, layer, or wrapper, apply the **deletion test**: preserving required behavior, would removing it eliminate complexity or move that complexity into callers? Account for concrete adaptation and compatibility duties. Justify interface elements by what callers gain relative to what they must learn.
- When touching an API, check whether ordinary callers must know sequencing, representation, storage, transport, caching, protocol, file format, internal workflow, or too many setup steps.
- When adding a special case, flag, exception path, conditional, or exposed container, first ask whether the owning module can eliminate the invalid state, isolate the unusual behavior, or provide a stronger operation.
- When splitting, extracting, or introducing variables, check whether the new boundary or name captures meaning or only adds jumps, pass-through state, and visible intermediate steps.
- When code is organized as `prepare/process/finalize`, staged objects, or other execution-order phases, verify that temporal structure is the real concept; otherwise reorganize around stable responsibilities.
- When naming is vague, mechanism-focused, inconsistent, or surprising, reconsider the abstraction boundary instead of accepting a near miss.
- When comments are long or hard to write, distinguish necessary contract detail from confusing design. Redesign tangled abstractions, move caller-needed constraints to the interface, and remove code narration; length alone does not establish a defect.
- When optimizing performance, measure first and hide the optimization; do not sacrifice module depth or information hiding without evidence that the tradeoff matters.
- When testing or reviewing, focus on public behavior, interface contracts, hidden complexity through stable APIs, and special cases isolated behind the abstraction. After restructuring, remove tests tied to discarded internals only when distinct behavior coverage remains.

## Final checklist

Use only the items relevant to the changed design.

- Does the change meet its requirements without avoidable complexity, and is necessary added complexity justified?
- Do the touched interfaces and layers justify their learning cost through hidden complexity or a necessary adaptation or compatibility role?
- Are important decisions localized, dependencies visible, caller-needed constraints documented, and mutable internals protected?
- Are common cases simple, unusual behavior contained, and failures that matter to callers still explicit?
- Are names precise and consistent, comments current and non-duplicative, and conventions followed unless new information justified changing them?
