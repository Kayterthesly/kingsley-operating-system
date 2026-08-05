# WF-000 — Academic & Professional Identity Stack

Version: 0.1
Status: Draft — Section 4 of 6 complete
Author: Kingsley Akenu
Architect: Claude
Last Updated: 2026-08-04
Depends On: KOS-001 (System Architecture), KOS-003 (Career North Star)
Governs: WF-001–WF-006 (proposed, not yet created) and any future workflow document that creates or modifies a public-facing artifact

---

## Document Map

1. Purpose & Problem Statement — **complete**
2. Scope & Definitions — **complete**
3. Identity Artifact Registry — **complete**
4. Synchronization Model — **complete**
5. Governance & Versioning — pending
6. Relationship to the WF-00X Ecosystem — pending

---

# 1. Purpose & Problem Statement

## Purpose

WF-000 establishes a single governing definition of what constitutes the professional identity stack, assigns each artifact in that stack a canonical source of truth, and defines the conditions under which artifacts are considered synchronized versus drifted.

Its function is preventive, not narrative. It exists so that a change made in one place — a new project, a corrected metric, an updated role, a renamed link — has a defined path to every other place that claim appears, instead of relying on memory or ad hoc edits. Every downstream WF-00X document that creates or modifies a public-facing artifact (resume, GitHub, portfolio, LinkedIn, etc.) inherits its definitions and constraints from this document rather than re-defining them independently.

## Scope

**In scope for this section:** a working definition of identity-stack drift, and the evidence-based case for why a governing document is warranted given the current state of the stack.

**Out of scope for this section** (deferred to later sections):
- The enumerated list of artifacts and their canonical sources — Section 3
- The mechanics of how a sync is triggered, executed, and verified — Section 4
- How WF-000 itself is versioned and who can approve changes to it — Section 5

**Explicitly out of scope for WF-000 as a whole:**
- Narrative CV content or resume copywriting — that is execution work, not governance, and belongs to the artifact itself
- Database or storage schema — that is KOS-002
- Career strategy or goal-setting — that is KOS-003

## Inputs

- KOS-001 — System Architecture, specifically Principle 2 ("Everything Connects") and Principle 4 ("Document Everything"), which are the philosophical basis for this document existing at all
- KOS-003 — Career North Star, which defines the long-horizon goals the identity stack exists to serve
- The current resume (Kingsley_Akenu_Resume_Updated (1).docx), used here as the first concrete evidence source
- The WF-001–006 proposal, which establishes that multiple future documents will each generate or modify public artifacts and therefore need a shared contract so they don't contradict one another

## Outputs

A working definition that Sections 2–6 and all future WF-00X documents can reference without re-deriving it:

> **Identity-stack drift**: a state in which two or more public-facing artifacts make claims about the same underlying fact (a project, a metric, a role, a link, a status) that are inconsistent, outdated, or unverifiable relative to one another.

## Dependencies

This section demonstrates that identity-stack drift is a real operational risk using one current example, documented below. Future instances of drift should be tracked within the affected artifact or repository, not within WF-000.

**Current example of drift (resume, "Flagship Project 1" block):** the paragraph describing the healthcare readmission pipeline is immediately followed by three unlabeled hyperlinks — Dashboard / API Health / GitHub Source — that resolve to the cryptocurrency pipeline, not the healthcare pipeline. Nothing in the artifact signals that these three links belong to a different project than the text directly above them. A reader has every reason to assume they are healthcare links and no signal that they are not. This is drift by the definition above: the artifact's structure implies one claim while its content delivers another.

## Acceptance Criteria

1. The problem statement is falsifiable against a real, current example — not a hypothetical.
2. A reader with no prior context on this repository can read this section alone and understand why WF-000 needs to exist.
3. The definition of "identity-stack drift" is specific enough to be used as a pass/fail test in Section 4 — not just a general concern.
4. Nothing in this section promises a capability (e.g., "synchronization") that a later section doesn't go on to define a mechanism for.

## Verification Checklist

- [x] At least one real drift example is cited and independently checkable against the source artifact
- [x] The definition of "identity-stack drift" does not rely on subjective judgment alone — it can be checked against artifact contents directly
- [x] Purpose statement makes no unfulfilled promise (synchronization is named as a goal but not claimed as solved here — that is Section 4's job)
- [x] Cross-references to KOS-001 and KOS-003 are accurate to those documents as currently written
- [x] Confirmed by Kingsley before Section 2 begins

---

**Section 1 of 6 complete. Approved 2026-08-04.**

---

# 2. Scope & Definitions

## Purpose

Section 2 sets the operating boundary for WF-000 as a whole and defines the vocabulary used consistently across Sections 3–6 and every downstream WF-00X document. Two failure modes motivate it. First, without an explicit boundary, "identity stack" can expand to include anything loosely connected to Kingsley's career — the exact scope-creep risk this document exists to guard against, including within itself. Second, without shared definitions, terms like "canonical source" or "sync event" would be re-defined ad hoc in every downstream document, which defeats the purpose of having a governing document at all.

## Scope

**In scope for this section:** a document-wide scope boundary (what "identity stack" does and does not include, for the purposes of WF-000), and a glossary of terms used across the rest of the document.

**Out of scope for this section** (deferred to later sections):
- The named, enumerated list of specific artifacts (which repos, which profiles) — Section 3
- The specific triggers and verification steps for a sync event — Section 4

## Inputs

- Section 1's definition of identity-stack drift, which this section must extend, not contradict
- KOS-003 (Career North Star), used to test whether categories of content near the boundary (e.g., research writing, open-source contributions) belong inside or outside the stack
- The WF-001–006 proposal, since its planned deliverables need to sit cleanly inside or outside this boundary without a special exception
- The existing KOS-00X and WF-00X documents themselves, used as the boundary's hardest test case (see below)

## Outputs

### Document Scope Statement

**In scope:** any durable, public-facing surface that makes a claim — explicit or implied — about Kingsley's work, skills, experience, or achievements to an external audience (employers, recruiters, professors, admissions committees, collaborators). This includes the resume, GitHub project profiles and READMEs, the portfolio website (once built), LinkedIn, and published technical writing.

**Out of scope:** internal planning and architecture documents — the KOS-00X and WF-00X series, including this document. These inform the identity stack but are not themselves claims made to an external audience; they are process artifacts, not identity artifacts. This exclusion is what stops WF-000 from having to govern itself, which would produce an infinite regress.

**Boundary case, resolved:** if the KOS repository itself is made public, its existence and visibility become an identity signal — "this person documents rigorously" — and belong in Section 3's registry as an artifact in its own right. Its internal contents remain process artifacts. WF-000 governs whether the repository is listed and linked correctly elsewhere; it does not govern the accuracy or completeness of what's written inside it.

Applied to specific candidates:

| Candidate | In scope? | Reasoning |
|---|---|---|
| Resume | Yes | Public-facing, makes verifiable claims to an external audience |
| Project GitHub repos / READMEs | Yes | Public-facing, makes claims about what was built and how |
| LinkedIn profile | Yes | Public-facing, makes claims about roles and experience |
| Portfolio website (future) | Yes | Same category as above, once it exists |
| KOS-00X / WF-00X documents | No | Process artifacts — inform the stack, aren't part of it |
| KOS repository, if made public | Boundary case | Existence/visibility in scope; internal contents out of scope |
| A one-off social post | No, by default | Not durable or intended as a standing claim, unless explicitly promoted into the Section 3 registry |

### Glossary of Terms

- **Identity Artifact** — any public-facing surface that makes a verifiable or falsifiable claim about Kingsley's work, skills, experience, or achievements to an external audience.
- **Canonical Source** — for any given fact (a metric, a project description, a date, a link), the single artifact or record designated as authoritative. When artifacts disagree, the canonical source is correct by definition and the others are drifted.
- **Sync Event** — a defined trigger requiring a change to propagate from a canonical source to its dependent artifacts. Formally defined in Section 4.
- **Drift** — as defined in Section 1: inconsistency between two or more artifacts' claims about the same underlying fact.
- **Process Artifact** — internal planning or architecture documentation (KOS-00X, WF-00X) that informs the identity stack but is not itself an identity artifact, except where its own visibility becomes a claim (see boundary case above).
- **Governing Document** — a document, like WF-000, whose role is to constrain other documents rather than to itself function as an identity artifact.

## Dependencies

This section depends on Section 1's drift definition remaining stable — every term here is built to be consistent with it, not a replacement for it. It also depends on having at least one genuinely ambiguous case to test the boundary against, rather than only listing clean examples; the "KOS repository made public" case serves that role, since a naive scope definition ("anything public") would incorrectly pull all of WF-000's own contents into its own jurisdiction.

## Acceptance Criteria

1. Every term used in Sections 3–6, as currently planned, is defined here with no forward reference to an undefined term.
2. The scope boundary can be applied to a new candidate artifact directly, without further interpretation.
3. WF-000 explicitly excludes the KOS-00X/WF-00X series from self-governance, preventing infinite regress.
4. At least one non-obvious boundary case is resolved explicitly rather than left implicit.

## Verification Checklist

- [x] Glossary covers every term used in Section 1 and in the Section 3–6 titles in the Document Map
- [x] Scope statement explicitly excludes KOS-00X/WF-00X documents from self-governance
- [x] The repository-visibility boundary case is resolved explicitly, not left ambiguous
- [x] No glossary term is defined using itself
- [x] Scope test table gives a directly reusable answer for at least one candidate not already named in Section 1
- [x] Confirmed by Kingsley before Section 3 begins

---

**Section 2 of 6 complete. Approved 2026-08-04.**

---

# 3. Identity Artifact Registry

## Purpose

Section 3 converts Section 2's scope boundary into a concrete, named list: every artifact currently inside that boundary, its single canonical source, its owner, and the class of event that should make someone check it for drift. This is the object the rest of the document exists to protect. Section 4 will define how synchronization is actually verified, but it can only operate on artifacts this section has already named and resolved to one canonical source.

## Scope

**In scope for this section:** naming every identity artifact currently identifiable from the project's files and the resume's own content; assigning exactly one canonical source and one owner per artifact; stating the class of event that should trigger a drift check.

**Out of scope for this section** (deferred to later sections):
- How a sync is actually executed, automated, or verified — Section 4
- How this registry itself is amended when an artifact is added, retired, or reassigned — Section 5

## Inputs

- Section 2's scope boundary and glossary (Identity Artifact, Canonical Source, Sync Event, Process Artifact)
- The resume (`Kingsley_Akenu_Resume_Updated (1).docx`), read directly rather than assumed
- `Kingsley_Akenu_Resume_2.pdf`, read directly rather than assumed — see findings below
- The project's actual file listing on disk, used as ground truth for what artifacts currently exist, rather than relying on any single document's self-description

## Outputs

### Artifact 1 — Resume

Two files currently exist in the project claiming to be the resume:

1. `Kingsley_Akenu_Resume_Updated (1).docx`
2. `Kingsley_Akenu_Resume_2.pdf`

Direct inspection of both — not just their filenames — resolves which is canonical:

- **`Kingsley_Akenu_Resume_Updated (1).docx`** is the current version. It includes the healthcare readmission pipeline as the flagship project, the fuller Core Competencies list (tidymodels, XGBoost, glmnet, RAG/Gemini, governance audit trails), and both project write-ups (healthcare and crypto) under Notable Projects.
- **`Kingsley_Akenu_Resume_2.pdf`** is a stale, earlier snapshot, not a duplicate. Its Professional Summary omits Healthcare entirely ("Retail, E-commerce, Finance, and Cryptocurrency markets"), its Flagship Project is the crypto pipeline, and its Notable Projects section has no healthcare entry at all — it predates that project's addition to the resume.

There is a second, independent problem with this file: **despite the `.pdf` extension, it is not a valid PDF.** Direct inspection shows it is a ZIP archive containing two page images, two OCR text files, and a manifest — not PDF file structure. A PDF reader, an applicant tracking system, or a recruiter's file-type filter would likely fail to open it as expected.

This also explains the Section 1 finding. The most likely sequence: the resume was originally built around the crypto pipeline as the flagship project — matching this older file — including its three "Dashboard / API Health / GitHub Source" links. When the healthcare project was added and promoted to flagship, the paragraph above those links was rewritten, but the three links themselves were never removed or repointed. What Section 1 documented as an isolated formatting error is more precisely the residue of this file's older content never being fully retired.

| Field | Value |
|---|---|
| Canonical Source | `Kingsley_Akenu_Resume_Updated (1).docx` |
| Owner | Kingsley Akenu |
| Sync Trigger | Any change to the project portfolio, a metric, a role, or a date that would alter a claim currently made in the resume — including retirement of `Kingsley_Akenu_Resume_2.pdf` itself, which should be treated as the first sync action once Section 4's mechanics exist |

### Artifacts 2–10 — Profiles and Live Project Endpoints

| # | Artifact | Location | Canonical Source | Owner | Sync Trigger |
|---|---|---|---|---|---|
| 2 | GitHub Profile | github.com/Kayterthesly | The live profile itself | Kingsley Akenu | A repo is added, removed, or renamed; bio or pinned repos change |
| 3 | LinkedIn Profile | linkedin.com/in/kayterthesly | The live profile itself | Kingsley Akenu | Role, certification, or project summary changes |
| 4 | Portfolio Site (current) | kayterthesly.github.io/Kayterthesly | The live deployed site | Kingsley Akenu | A project is completed, the resume changes, or a repo is added |
| 5 | Healthcare Pipeline — GitHub Repo | github.com/Kayterthesly/r-healthcare-readmission | The repository (code + README) | Kingsley Akenu | Any code, test, or deployment change that alters what the README claims |
| 6 | Healthcare Pipeline — Live Dashboard | e9yw5n-kayterthesly.shinyapps.io/healthcare-readmission-pipeline/ | The live deployment | Kingsley Akenu | Redeploy, or the service is moved or retired |
| 7 | Healthcare Pipeline — Live API | r-healthcare-readmission-production.up.railway.app/health | The live deployment | Kingsley Akenu | Redeploy, or the service is moved or retired |
| 8 | Crypto Pipeline — GitHub Repo | github.com/Kayterthesly/crypto-price-pipeline | The repository (code + README) | Kingsley Akenu | Any code, test, or deployment change that alters what the README claims |
| 9 | Crypto Pipeline — Live Dashboard | e9yw5n-kayterthesly.shinyapps.io/crypto-price-pipeline/ | The live deployment | Kingsley Akenu | Redeploy, or the service is moved or retired |
| 10 | Crypto Pipeline — Live API | crypto-price-pipeline-production.up.railway.app/health | The live deployment | Kingsley Akenu | Redeploy, or the service is moved or retired |

### Artifacts 11–12 — Conditional / Reserved

| # | Artifact | Location | Canonical Source | Owner | Sync Trigger |
|---|---|---|---|---|---|
| 11 | KOS Repository | Unconfirmed — public status unknown | N/A until public (per Section 2 boundary case) | Kingsley Akenu | Activates only if/when the repository is made public |
| 12 | Published Technical Writing | No current instance | N/A — reserved category | Kingsley Akenu | Activates on first publication (blog post, article, paper) |

## Dependencies

This section depends on Section 2's glossary for "Identity Artifact" and "Canonical Source." It does not depend on a glossary entry for "Owner" — that term is used here in its plain sense ("the person accountable for keeping this artifact's claims accurate"), not as specialized vocabulary, so its absence from Section 2 is not a gap requiring amendment.

It also depends on every artifact having a uniform owner: Kingsley Akenu. That uniformity is a fact about the current stack, not a placeholder — it's what makes a single Sync Trigger column sufficient without a separate approver/reviewer field. If a collaborator or co-author is ever added to any artifact, this registry would need that distinction added; Section 5 (Governance & Versioning) is the natural place to define how such an amendment happens.

Finally, this section depends on direct inspection of both resume files rather than trusting their filenames or extensions — `Kingsley_Akenu_Resume_2.pdf` identifies itself as a PDF but is not one internally. That distinction was only found by opening the file, consistent with KOS-001 Principle 7, "Evidence Over Assumptions."

## Acceptance Criteria

1. Every artifact identifiable from the project's actual files and the resume's own content is named — not a representative sample.
2. Every artifact has exactly one canonical source. No artifact is left with two competing "canonical" claims.
3. Where two candidate sources conflicted (the two resume files), the conflict is resolved with direct evidence, not resolved by default or assumption.
4. No row describes synchronization mechanics — only the triggering condition.
5. Every artifact has a named owner, and the uniformity of that value across the registry is explained rather than left unstated.

## Verification Checklist

- [x] Every artifact from the resume, the project's file listing, and the two-resume-file discrepancy is represented
- [x] Exactly one canonical source is assigned per artifact
- [x] The docx-vs-PDF conflict is resolved using direct file inspection, not assumption
- [x] `Kingsley_Akenu_Resume_2.pdf`'s underlying format was independently verified (not a valid PDF) rather than taken on faith from its extension
- [x] No entry describes automation, scripts, or CI mechanics — only trigger conditions
- [x] "Owner" usage is checked against Section 2's glossary; the non-gap is explained
- [x] Confirmed by Kingsley before Section 4 begins

---

**Section 3 of 6 complete. Approved 2026-08-04.**

---

# 4. Synchronization Model

## Purpose

Section 4 defines how the registry from Section 3 stays accurate over time: what class of event should prompt a check, the exact test for whether an artifact has drifted, the workflow that runs from trigger to resolution, and which role in that workflow is responsible for each step. It is deliberately silent on tooling — no automation platform, script, or service is named — so the model stays valid whether it's executed by hand, by script, or by something adopted later, and doesn't need to be rewritten when the tooling changes.

## Scope

**In scope for this section:** a trigger taxonomy abstracted from Section 3's per-artifact triggers, a drift-detection test, a verification workflow, and the roles that carry it out.

**Out of scope for this section:**
- Any specific tool, platform, script, or automation product — excluded by this section's own mandate
- How this model itself gets amended over time — Section 5
- How WF-000 relates to WF-001–006 — Section 6

## Inputs

- Section 3's registry — 10 confirmed artifacts plus 2 conditional entries, each already assigned a canonical source and a per-artifact trigger
- Section 1's definition of drift, and its one confirmed real example (the healthcare/crypto link mismatch), used below to validate this section's model rather than just assert it
- Section 2's convention that "Owner" is used in its plain sense, extended here into named workflow roles

## Outputs

### Trigger Taxonomy

Section 3 assigned a specific trigger to each artifact individually. Read together, those triggers fall into four general classes:

| Trigger Class | Definition | Example from the Registry |
|---|---|---|
| Fact Change | The underlying truth about a project, role, metric, or credential changes | A new project ships; a metric is corrected; a role ends (Resume, LinkedIn) |
| Deployment Change | A live system's actual state changes, independent of any narrative describing it | Redeploy, move, or retirement of a dashboard or API (Artifacts 5–10) |
| Registry Change | The set of tracked artifacts itself changes | A repo is added or renamed (GitHub Profile); an artifact is retired (`Kingsley_Akenu_Resume_2.pdf`) |
| Scheduled Review | A fixed interval passes, regardless of whether any other trigger fired | Catches drift with no discrete triggering event — Review cadence is governed by Section 5 and may vary by artifact type. |

Every trigger in Section 3's registry falls into one of the first three classes. The fourth exists because event-driven triggers only catch drift someone notices happening — Scheduled Review is the backstop for drift that accumulates silently, the way the crypto/healthcare link mismatch apparently did.

### Drift Detection Procedure

Given a fact **F** and the canonical source assigned to it in Section 3:

1. Identify every registry artifact whose content could plausibly make a claim about F.
2. For each one, extract its current claim about F.
3. Compare that claim against the canonical source's current value for F.
4. Classify the result:
   - **Synced** — the claim matches the canonical value.
   - **Drifted** — a claim exists and contradicts the canonical value.
   - **Gap** — the artifact makes no claim about F at all.

Drift and Gap are different problems with different fixes. Drift means an artifact is actively wrong and needs correcting. Gap means an artifact is silent, which may be fine (not everything needs to mention everything) or may need a decision — but it is not automatically an error the way Drift is.

**Worked example — validating the procedure against the known Section 1 case:**

| Step | Value |
|---|---|
| F | Which live links follow the Flagship Project description in the resume |
| Canonical source | The r-healthcare-readmission project's actual live links (Section 3, Artifacts 5–7) — healthcare is the current flagship project |
| Dependent artifact | The resume's "Flagship Project 1" block |
| Dependent claim | Crypto-pipeline links appear immediately after the healthcare description |
| Comparison | Dependent claim ≠ canonical value |
| Classification | **Drifted** |

The procedure reproduces the finding Section 1 made by inspection alone. That's the actual test of whether this section is doing new work or just restating Section 1 in more formal language — it passes.

### Verification Workflow

1. **Trigger fires** — one of the four classes above occurs.
2. **Scope the fact** — identify the specific claim the trigger concerns, not the whole artifact.
3. **Pull canonical value** — check the canonical source assigned in Section 3.
4. **Check dependents** — apply the Drift Detection Procedure to every artifact that could plausibly reference that fact.
5. **Classify** — Synced / Drifted / Gap, per artifact checked.
6. **Resolve** — correct anything Drifted so it matches canonical truth. (If the canonical source itself is wrong, fix it first, then re-run steps 3–5 before propagating further.) For a Gap, make a decision rather than defaulting to silence.
7. **Record the check** — note when the check occurred, what was checked, what was found, what changed, and who performed the verification. The form of this record isn't specified here — a changelog line, a note, a commit message all satisfy it — only that a check which isn't recorded is indistinguishable from a check that never happened.
8. **Close** — the trigger is resolved once every affected dependent is Synced, or its Gap was a deliberate decision rather than an oversight.

### Roles & Ownership within the Workflow

| Role | Responsible For | Currently |
|---|---|---|
| Initiator | Noticing or declaring that a trigger has fired | Kingsley Akenu |
| Verifier | Steps 2–5: scoping the fact, pulling canonical value, checking dependents, classifying | Kingsley Akenu |
| Resolver | Step 6: making the actual correction | Kingsley Akenu |
| Approver | Steps 7–8: confirming the resolution is correct and closing the check | Kingsley Akenu |

All four roles currently resolve to one person. They're kept distinct anyway, because that's what lets a future collaborator take on a single role for a single artifact — a co-author signing off as Approver on a shared publication, for instance — without this section needing to be restructured. How a role reassignment would actually be approved belongs to Section 5, not this one.

## Dependencies

This model depends on Section 3's canonical-source assignments being correct. If a canonical source is itself wrong, this model will faithfully propagate that wrong value to every dependent artifact — drift detection only checks consistency with the canonical source, not the canonical source's own accuracy. That's a real limitation, not an oversight: verifying a canonical source's own correctness is closer to fact-checking the underlying project claims, and is out of scope for a synchronization model.

It also depends on the four workflow roles remaining conceptually separable even while collapsed into one person today — the model doesn't require multiple people, only that the roles don't need to be invented later if that changes.

## Acceptance Criteria

1. Every trigger class is derived from patterns actually present in Section 3's registry, not invented in the abstract.
2. The Drift Detection Procedure is precise enough to apply by hand to a real case without further interpretation.
3. The procedure is validated against the one confirmed real drift case from Section 1 and correctly classifies it.
4. No tool, platform, script, or automation product is named anywhere in this section.
5. Workflow roles are functionally distinct even though currently assigned to one person.
6. The model states what it cannot do (verify a canonical source's own correctness) rather than implying it catches everything.

## Verification Checklist

- [x] Trigger taxonomy covers every trigger type present in Section 3's registry
- [x] Drift Detection Procedure distinguishes "Drifted" from "Gap" as separate conditions
- [x] The procedure is run against the real Section 1 case and correctly returns "Drifted"
- [x] Verification Workflow forms a closed loop: trigger → resolution → record → close
- [x] No tooling, platform, or automation product is named anywhere in the section
- [x] Roles table shows four distinct functions even though currently one owner
- [ ] Confirmed by Kingsley before Section 5 begins

---

**Section 4 of 6 complete.**