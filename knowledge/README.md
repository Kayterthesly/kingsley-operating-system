# Knowledge Layer

Version: 1.0
Status: Active

---

# Purpose

The Knowledge Layer stores reusable intellectual assets extracted from completed work.

Unlike project documentation, knowledge assets are intentionally project-independent.

Their purpose is to preserve techniques, decision frameworks, engineering practices, and reusable patterns that can improve future work.

---

# Philosophy

Projects are temporary.

Knowledge compounds.

Every completed project should leave behind reusable intellectual assets that make future projects better.

Projects create experience.

Knowledge preserves experience.

---

# Repository Structure

```
knowledge/

    engineering-patterns/

    architecture-patterns/

    deployment-patterns/

    debugging-patterns/

    decision-frameworks/

    research-notes/

    reference-models/
```

---

# Sources

Knowledge assets are extracted from:

- completed workflows

- completed projects

- research

- production incidents

- experimentation

- architectural decisions

---

# Relationship to Workflows

Workflow documents explain

> how one project happened.

Knowledge documents explain

> how future projects should benefit.

For example:

```
WF-001

↓

Stage 4 Debugging

↓

Engineering Pattern

↓

knowledge/engineering-patterns/
```

---

# Quality Standard

Knowledge should be

- reusable

- evidence-based

- technology-agnostic where possible

- concise

- version controlled

---

# Rule

If something is likely to help twice,

it belongs here.