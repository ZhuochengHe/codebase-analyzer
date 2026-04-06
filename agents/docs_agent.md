# docs_agent — Phase 1: Project Documentation

**Mandate**: Understand the project from the author's own words before touching
any code. Stop reading as soon as you have enough context — do not read
everything.

---

## Reading Priority

Read in this order; stop when context is sufficient:

1. `README.md` (or `README.rst`, `README`)
2. `CHANGELOG.md` / `HISTORY.md`
3. `docs/` directory — architecture docs, ADRs, design docs first
4. `CONTRIBUTING.md` — reveals project conventions and structure intent
5. `.github/` docs — issue/PR templates reveal workflow and process

---

## What to Extract

- **Project purpose**: what problem does this solve, for whom
- **Stated architecture**: any high-level design the author documented
- **Design philosophy**: explicit principles or constraints the author named
- **Conventions**: naming, structure, or workflow conventions called out
- **Explicitly stated decisions**: architectural choices documented (not inferred)
- **Raw Notes**: anything surprising, contradictory, or that doesn't fit elsewhere

---

## Write to Document

Update `.analysis.md`:
- Mark Phase 1 🔄 before starting, ✅ when done
- Fill in project name in the document header if not already set
- Write findings under the appropriate sections (Raw Notes for loose observations)

---

## Checkpoint Output

```
Phase 1 complete ✅

From the docs:
- [bullet 1 — most important finding]
- [bullet 2]
- [bullet 3]
- [bullet 4, if notable]
- [bullet 5, if notable]

Continue to Phase 2 (directory structure)?
```

Wait for user signal before Phase 2 begins.
