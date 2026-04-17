---
name: consolidate-docs
description: Merge time-series PRDs and child PRDs into a single time-neutral document that reads as if the system always worked this way. Use after PRD iterations, before handoff, or when cleaning up docs.
---

# Consolidate Docs

## When to Use

- After multiple PRD iterations on the same feature
- Before a major release or handoff to another person/session
- When docs folder has ≥2 related PRDs covering the same feature area
- When someone asks to "clean up the docs" or "update the spec"

## Principles

The consolidated document is **time-neutral**: it describes the current state of the system as if it always existed this way. No "previously", "we changed", "originally we planned", or "as of this iteration" language.

Time-series history belongs in git or an archive folder — not in the live spec.

## Workflow

### Step 1: Identify the Document Set

List all PRDs and child PRDs in the `docs/prds/` folder. Group by feature area.

### Step 2: Identify "Final State"

For each group, determine:
- What does the system actually do today?
- Which decisions from the PRD history are still in effect?
- What was proposed but never built? (exclude)
- What was built differently from what was proposed? (use actual, not planned)

### Step 3: Write Consolidated Document

Create: `docs/prds/<feature-area>-current.md`

Rules for this document:
- Present tense throughout ("The system stores X as Y" not "We decided to store")
- No version numbers or dates in body text
- No references to previous approaches that were abandoned
- Every statement describes current reality, not intent or history
- Sections: Overview, Architecture, Data Model, API/Interface, Error Handling, Known Limitations

### Step 4: Archive Originals

Move source documents to `docs/prds/archive/`:

```bash
mkdir -p docs/prds/archive
mv docs/prds/YYYY-MM-DD-*.md docs/prds/archive/
```

Keep the consolidated document as the single source of truth.

### Step 5: Update References

Search codebase for references to archived PRD filenames and update them to point to the consolidated document.

```bash
grep -r "docs/prds/YYYY-MM-DD" . --include="*.md"
```

### Step 6: Advisor Gate

Invoke `advisor-gate` completion gate to confirm consolidation is accurate and nothing was lost.
