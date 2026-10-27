# ADR Format

ADRs live in `docs/adr/` and use sequential numbering: `0001-slug.md`, `0002-slug.md`, etc. Create the directory lazily when the first ADR is needed. Scan it for the highest existing number and increment by one.

## Template

```md
---
status: accepted
---

# {Short title of the decision}

{1-3 sentences: what's the context, what did we decide, and why.}
```

An ADR can be a single paragraph. Add **Considered Options** only when rejected alternatives are worth remembering, and **Consequences** only when non-obvious downstream effects need to be called out.
