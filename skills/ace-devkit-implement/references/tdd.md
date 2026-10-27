# Test-Driven Development

TDD is the red → green loop. This document is the reference that makes that loop produce tests worth keeping: what a good test is, where tests go, the anti-patterns, and the rules of the loop. Every section applies on every cycle: consult them before and during the loop, not after.

When exploring the codebase, read `GLOSSARY.md` (if it exists) so test names and interface vocabulary match the project's domain language, and respect ADRs in the area you're touching.

## What a good test is

Tests verify behavior through public interfaces, not implementation details. Code can change entirely; tests shouldn't. A good test reads like a specification: "user can checkout with valid cart" tells you exactly what capability exists, and it survives refactors because it doesn't care about internal structure.

See [tests.md](tests.md) for examples and [mocking.md](mocking.md) for mocking guidelines.

## Interfaces and seams: where tests go

Test through the public contract of the module under test, without reaching into its implementation. Internal modules can have their own interfaces and tests; those interfaces need not become part of the containing module's public API.

**Choose test interfaces from the confirmed design and verification strategy.** Before writing tests, identify the module interfaces under test. Give each test interface a one-line note on what it catches and what it misses. Choose implementation-level test interfaces and substitution seams yourself; ask the user only when the choice would change confirmed requirements, important design agreements, or an existing decision. Do not write tests that depend on that change until it is confirmed.

Apply the [codebase-design.md](../../shared/codebase-design/codebase-design.md) reference loaded at implementation start or resume when choosing or revising module interfaces, substitution seams, and tests. Keep the design rules there; testing through an interface does not by itself require a substitution seam or an adapter.

## Anti-patterns

- **Implementation-coupled**: mocks internal collaborators, tests private methods, or verifies through a side channel (querying the database instead of using the interface). The tell: the test breaks when you refactor but behavior hasn't changed.
- **Tautological**: the assertion recomputes the expected value the way the code does (`expect(add(a, b)).toBe(a + b)`, a snapshot derived by hand the same way, a constant asserted equal to itself), so it passes by construction and can never disagree with the code. Expected values must come from an independent source of truth: a known-good literal, a worked example, the spec.
- **Horizontal slicing**: writing all tests first, then all implementation. Bulk tests verify _imagined_ behavior: you test the _shape_ of things rather than user-facing behavior, the tests go insensitive to real changes, and you commit to test structure before understanding the implementation. Work in **vertical slices** instead: one test → one implementation → repeat, each test a **tracer bullet** that responds to what the last cycle taught you.

## Rules of the loop

- **Red before green.** Write the failing test first, then only enough code to pass it. Don't anticipate future tests or add speculative features.
- **One slice at a time.** One interface, one test, one minimal implementation per cycle.
- **Refactoring is not part of the loop.** It belongs to the review stage (see [code-review.md](code-review.md)), not the red → green implementation cycle.
