# WF-000 — Academic & Professional Identity Stack

Version: 1.0
Status: Released
Author: Kingsley Akenu
Architect: Claude
Last Updated: 2026-08-05
Depends On: KOS-001 (System Architecture), KOS-003 (Career North Star)
Governs: WF-001–WF-006 (proposed, not yet created) and any future workflow document that creates or modifies a public-facing artifact

---

## Document Map

1. Purpose & Problem Statement — **complete**
2. Scope & Definitions — **complete**
3. Identity Artifact Registry — **complete**
4. Synchronization Model — **complete**
5. Governance & Versioning — **complete**
6. Relationship to the WF-00X Ecosystem — **complete**

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
| Scheduled Review | A fixed interval passes, regardless of whether any other trigger fired | Catches drift with no discrete triggering event — review cadence is governed by Section 5 and may vary by artifact type |

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
- [x] Confirmed by Kingsley before Section 5 begins

---

**Section 4 of 6 complete. Approved 2026-08-05, incorporating two refinements.**

**Refinement log — 2026-08-05:** two changes proposed by Kingsley and accepted, applied directly to the Section 4 text above:
1. Verification Workflow, Step 7 now records when a check occurred and who performed it, in addition to what was checked, found, and changed.
2. The Scheduled Review trigger no longer proposes a fixed quarterly cadence; cadence is deferred to Section 5, which may vary by artifact type (resolved in Section 5.3 below).

Per the versioning rule defined in Section 5.1, amending an already-approved section bumps the document's minor version. This edit moves WF-000 from Version 0.1 to **Version 0.2**, reflected in the front matter above.

---

# 5. Governance & Versioning

## Purpose

Section 5 defines how WF-000 itself changes over time without becoming unreliable: what a version number means, who has final say over a change, how often each type of artifact in the registry gets reviewed, how a proposed change moves from suggestion to accepted text, and what has to be true before this document can call itself stable. It's written partly in response to something that already happened: the two refinements just applied to Section 4 occurred before any rule existed for how such a change should be handled. This section formalizes a process that has already been used once, informally, rather than inventing one from scratch.

## Scope

**In scope for this section:** document-level versioning for WF-000 and, by the same convention, the WF-00X and KOS-00X series; approval authority; artifact review cadence by type (resolving Section 4's deferral); the amendment lifecycle; stability criteria for Version 1.0.

**Out of scope for this section:**
- How WF-000 specifically relates to and constrains WF-001–006 — Section 6
- Any Git platform, project management tool, or software product — excluded by this section's own mandate

## Inputs

- Section 4's explicit deferral of review-cadence policy to this section, reinforced by Kingsley's second refinement above
- The two refinements just applied to Section 4, used below as the worked example for the amendment process
- Section 3's artifact registry and its categories, used as the basis for assigning cadence by type
- KOS-001, Principle 6 ("Kaizen — improve a little every day"), which is what versioning and amendment exist to formally capture

## Outputs

### 5.1 Document Versioning

Applies to governing documents — WF-000, and by the same convention the WF-00X and KOS-00X series. It does not apply to identity artifacts: the resume does not get a version number; its currency is handled by the review cadence in 5.3 and the canonical-source model in Section 4, not by this scheme.

A two-part number, MAJOR.MINOR:

- **0.x — Draft.** The document has not yet met the stability criteria in 5.5.
  - **0.1** marks the first complete pass through every planned section — reaching it means the document exists in full for the first time, not that it's finished being refined.
  - Each further **minor bump** (0.2, 0.3, …) marks a revision cycle in which an already-approved section is amended, corrected, or refined after the fact.
- **1.0 — Stable.** Every criterion in 5.5 is met, and Kingsley has explicitly approved the bump.
- **Post-1.0 major bump** (2.0, 3.0, …) marks a structural change: a section added, removed, or reordered, or a rule from an earlier section redefined in a way that changes what a downstream document (WF-001–006) can assume.

**Applying the rule to what already happened:** the two refinements to Section 4 are the first instance of amending an already-approved section. Per the rule above, that makes this document's actual version, as of this edit, Version 0.2 — reflected in the front matter.

### 5.2 Approval Authority

Two rights, kept separate because they've already been exercised differently in this document's own history:

- **Proposal rights** — Kingsley, or whoever is acting as architect/writer for a given document, may propose new content, a correction, or a refinement. Both of today's refinements to Section 4 were proposals of this kind.
- **Approval authority** — Kingsley Akenu, exclusively. No section, artifact entry, or governance rule takes effect until he confirms it. Every section so far has ended on the same checklist item — "Confirmed by Kingsley before Section N begins" — and none has been treated as final without it being checked.

This mirrors the Approver role defined at the artifact level in Section 4.4: the same person who signs off on a resolved drift also signs off on a change to the rules themselves. A governing document with an audience of one doesn't need a review board — it needs one consistently-applied confirmation step, which is already in place and doesn't need to be invented here.

### 5.3 Artifact Review Cadence by Type

Section 4 deferred this by design; Kingsley's second refinement made the deferral explicit: cadence is governed here and may vary by artifact type. Applied to Section 3's registry:

| Artifact Type | Cadence | Rationale |
|---|---|---|
| Resume | Before every application, at minimum | Highest stakes — a new, one-time audience reads it each time it's used |
| LinkedIn / GitHub Profile | Monthly, or immediately on any Fact or Registry Change | Always visible; there's no natural "before it's read" checkpoint |
| Portfolio Site | Monthly, or immediately after the Resume changes | Makes the same claims as the Resume, to the same kind of audience — should never lag behind it |
| Live Dashboards & APIs | Monthly liveness/accuracy check | Can silently go down or drift with no visible signal until someone clicks the link |
| Project GitHub Repos / READMEs | On every Fact or Deployment Change, plus a light quarterly staleness check | Changes less often once a project stabilizes; the quarterly check catches what event triggers miss |
| Completed Project Write-ups (Notable Projects) | Occasional — annually, or on major revision to the underlying project | Describes finished work; nothing forces frequent review once it's accurate |
| Conditional / Reserved (KOS Repo, Published Writing) | Not applicable until activated | Per Section 3 — no cadence applies to an artifact that isn't active |

Every artifact type in the registry now has an assigned cadence; Section 4's deferral is fully resolved, not partially.

### 5.4 Amendment Process

1. **Propose** — state the specific change and why, in plain language. No ticket or platform is required; a clearly labeled statement is enough, as both refinements above demonstrated.
2. **Review** — check the proposal against three things: does it contradict a definition or decision already approved elsewhere in the document; does it stay inside Section 2's scope boundary; does it preserve tooling independence where that's required.
3. **Accept** — Kingsley confirms explicitly. Silence is not acceptance.
4. **Record** — log what changed, when, and who accepted it — the same discipline Section 4's Step 7 now requires of artifact-level checks, applied here to the document itself.

**Validating this against what already happened:** both refinements above followed exactly this sequence — proposed with a stated rationale, checked against the tooling-independence rule already in force, accepted explicitly ("Section 4 is approved"), and recorded via the refinement log and version bump. The process describes what this document has already done once; it wasn't designed in the abstract first.

### 5.5 Stability Criteria (Version 1.0)

WF-000 becomes eligible for Version 1.0 when all of the following hold:

1. Every section in the Document Map is complete and individually confirmed — no "pending" items remain.
2. Every forward reference an earlier section made to a later one has been resolved (Section 4's deferral of cadence policy, resolved in 5.3, is one example).
3. The document has been applied to at least one real case without requiring a structural change to fix it.
4. At least one revision cycle has occurred — a section amended after being marked complete, without breaking anything upstream or downstream of it.
5. Kingsley explicitly approves the bump. Meeting 1–4 makes the document *eligible*; it does not make the bump automatic, consistent with 5.2.

**Current status:** criteria 3 and 4 are already satisfied — Sections 1, 3, and 4 have each been tested against the real crypto/healthcare link mismatch and held up without restructuring, and today's Section 4 refinement is the first completed revision cycle. Criteria 1 and 2 are not yet satisfied: Section 6 remains, and it covers this document's literal reason for existing per Section 1 — its relationship to WF-001–006. WF-000 is not eligible for Version 1.0 until Section 6 is complete.

## Dependencies

This section depends on Section 3's artifact categories remaining the basis for cadence assignment in 5.3 — if the registry's shape changes materially, cadence-by-type should be revisited, not assumed to still fit. It also depends on Section 2's definition of Governing Document holding: the versioning scheme in 5.1 applies to documents like WF-000 that constrain other documents, not to identity artifacts, which are governed by cadence and canonical-source rules instead.

## Acceptance Criteria

1. The versioning rule is applied to a real, already-occurred event (today's Section 4 refinement), not only described in the abstract.
2. Every artifact type from Section 3 has an assigned review cadence — Section 4's deferral is fully resolved.
3. The amendment process is validated against the two refinements actually proposed, reviewed, and accepted earlier in this conversation.
4. Stability criteria for Version 1.0 are stated as checkable conditions, and WF-000's current status against them is reported honestly rather than rounded up.
5. No Git platform, project management tool, or software product is named anywhere in this section.
6. Document versioning (for governing documents) and artifact review cadence (for identity artifacts) are kept explicitly distinct.

## Verification Checklist

- [x] Versioning rule is defined and immediately applied — WF-000 moves from 0.1 to 0.2 in this same edit
- [x] Every Section 3 artifact type has an assigned cadence; none left unresolved
- [x] Amendment process matches, in retrospect, exactly what happened with today's two refinements
- [x] Stability criteria are checkable, and WF-000 is honestly assessed as not yet meeting all of them
- [x] No tool, platform, or software product is named anywhere in this section
- [x] Document-versioning vs. artifact-cadence distinction is stated explicitly
- [x] Confirmed by Kingsley before Section 6 begins

---

**Section 5 of 6 complete. Approved 2026-08-05.**

---

# 6. Relationship to the WF-00X Ecosystem

## Purpose

Section 6 defines WF-000's relationship to the WF-001–006 series: what those documents are for, what WF-000 governs versus leaves entirely to them, what they inherit automatically, and what they're free to decide for themselves. Section 1 named this relationship as WF-000's literal reason for existing — a governing document that nothing yet governs isn't finished, regardless of how complete its internal machinery is. Completing this section is also what allows WF-000 to close out the stability criteria defined in Section 5.

## Scope

**In scope for this section:** the stated purpose of WF-001–006; the governs-but-does-not-duplicate boundary; what downstream documents inherit; what they can and cannot override; one worked validation example; and the completion statement.

**Out of scope for this section:**
- Actually writing WF-001–006 — that is each document's own future work, not this section's
- Anything requiring a Git platform, project management tool, or software product to describe — excluded by this section's own mandate, consistent with Sections 4 and 5

## Inputs

- The original WF-001–006 proposal
- Section 3, Artifact 4 (Portfolio Site) — the existing registry entry used as the basis for the worked example
- Section 5.5's stability criteria, which this section's completion statement must audit against
- Section 2's Process Artifact / Governing Document distinction

## Outputs

### 6.1 Purpose of WF-001–006

| Document | Purpose (per the original proposal) | Specification Status |
|---|---|---|
| WF-001 — Healthcare Pipeline | Captures the operating model — objectives, constraints, architecture, success metrics, testing, deployment, governance — that made the already-completed healthcare readmission pipeline successful, as a template for the documents that follow it | Named; not yet written |
| WF-002 — Research Portfolio Website | A research-grade digital identity platform — landing page, about, research, projects, blog, case studies, publications, CV, timeline, and more — intended to support graduate applications, employer credibility, and research positioning | Proposed in detail; not yet written |
| WF-003 — Agentic Decision Intelligence Platform | A research platform combining agents, RAG, evaluation frameworks, and data engineering, described as a centerpiece of the identity stack alongside WF-002 | Proposed in detail; not yet written |
| WF-004 — Scholarship Application System | Named in the original proposal; purpose implied by its title but not elaborated beyond that | Named only; scope undefined |
| WF-005 — Research Writing Pipeline | Named in the original proposal; purpose implied by its title but not elaborated beyond that | Named only; scope undefined |
| WF-006 — Networking System | Named in the original proposal; purpose implied by its title but not elaborated beyond that | Named only; scope undefined |

WF-004–006 are represented honestly as under-specified rather than filled in with invented detail. WF-000 doesn't need their full scope to govern them — 6.2 defines what it governs regardless of what each document eventually contains.

### 6.2 Governs, Does Not Duplicate

WF-000 governs two things about every WF-00X document: any identity artifact that document creates or modifies once built, and that document's own compliance with the definitions, registry format, and approval process set out in Sections 2–5. WF-000 does not contain, summarize, or duplicate a WF-00X document's internal content — its architecture, technical stack, timeline, or subject-matter decisions belong entirely to that document.

A concrete boundary test: WF-003's choice of vector database, agent framework, or evaluation methodology is not WF-000's concern and will never appear here. The moment WF-003 produces something a recruiter, professor, or collaborator could see — a deployed demo, a published paper, a public repo — that thing becomes an identity artifact, gets a registry entry per Section 3's format, and is governed from there on exactly like every other artifact in this document. The platform's internal design never enters WF-000; its public footprint always does.

### 6.3 Inheritance

Every WF-00X document inherits the following from WF-000 automatically, without redefining it:

- The glossary from Section 2 (Identity Artifact, Canonical Source, Sync Event, Drift, Process Artifact, Governing Document)
- The Section 2 scope boundary — its own document is a Process Artifact, not an Identity Artifact, unless made public, in which case only its visibility, not its contents, becomes governed
- The Section 3 registry format (Artifact / Location / Canonical Source / Owner / Sync Trigger) for any new artifact it produces
- The Section 4 synchronization model in full — the trigger taxonomy, the Drift Detection Procedure, the Verification Workflow, and the four workflow roles
- The Section 5 governance model — the versioning scheme (applied to itself, starting at its own 0.1), the approval-authority split, and the amendment process

### 6.4 Override Boundaries

**Cannot override:**
- Any definition in Section 2's glossary
- The rule that a new public artifact must be registered — no WF-00X document may produce a public artifact and leave it untracked
- The Drift / Gap / Synced classification logic from Section 4
- The approval-authority model — Kingsley remains the sole approver of any WF-00X document's own governance content, exactly as for WF-000

**Can override or extend:**
- Its own internal technical content — architecture, stack, timeline, subject matter — entirely its own decision
- A review cadence for an artifact type not already covered by Section 5.3's table — proposed and added there, not invented as a private exception
- Additional artifact-level roles beyond Section 4.4's four, where a real collaborator is involved (a co-author as Approver on one shared publication, for instance) — this extends the model as designed, it doesn't bypass it

### 6.5 Worked Example — The Portfolio Site

Section 3, Artifact 4 already registers a real, current portfolio site: kayterthesly.github.io/Kayterthesly. WF-002 proposes a substantially larger Research Portfolio Website that would eventually replace it. Tracing that event through this document's own rules:

1. **Trigger:** the new site goes live — a Registry Change per Section 4's taxonomy, and the end of the current site's lifecycle as the canonical portfolio.
2. **Registration:** the new site is added to Section 3's registry using the existing format — a new Location, a canonical source (the new live deployment), an Owner (Kingsley, per the current uniform-ownership state), and a Sync Trigger following the same Deployment Change pattern already used for every other live artifact in the registry.
3. **Retirement:** the old entry is retired explicitly, not silently overwritten — the same pattern already established in Section 3 for `Kingsley_Akenu_Resume_2.pdf`.
4. **Cadence:** the new site inherits the existing "Portfolio Site" row from Section 5.3 (monthly, or immediately after the Resume changes) rather than requiring a new category, because it is the same artifact *type*, just a different implementation. This also tests whether 5.3's categories were defined at the right level of abstraction — they were, since changing implementation doesn't break the category.
5. **What WF-002 never has to decide:** what "canonical source" means, how drift against the Resume gets checked, or who approves the change. All of it is inherited.
6. **What WF-002 decides entirely on its own:** everything about how the site is actually built.

That's the relationship demonstrated rather than asserted: WF-002 plugs into a registry slot, a synchronization model, and an approval process it didn't have to invent, and keeps full control over everything WF-000 was never meant to touch.

### 6.6 Completion Statement

Auditing every forward reference made across Sections 1–5:

| Deferred In | To | Status |
|---|---|---|
| Section 1 | Artifact enumeration → Section 3 | Resolved |
| Section 1 | Sync mechanics → Section 4 | Resolved |
| Section 1 | Versioning & approval → Section 5 | Resolved |
| Section 2 | Artifact registry → Section 3 | Resolved |
| Section 2 | Sync triggers & verification → Section 4 | Resolved |
| Section 3 | Sync mechanics → Section 4 | Resolved |
| Section 3 | Registry amendment → Section 5 | Resolved |
| Section 4 | Model amendment → Section 5 | Resolved |
| Section 4 | Relationship to WF-001–006 → Section 6 | Resolved above |
| Section 5 | Relationship to WF-001–006 → Section 6 | Resolved above |

Every forward reference made by an earlier section is accounted for. Combined with the Document Map now showing all six sections complete, **Section 5.5's criteria 1 and 2 are satisfied.** Criteria 3 and 4 were already satisfied as of Section 5. That leaves criterion 5 — Kingsley's explicit approval of the version bump itself — as the only remaining condition for Version 1.0.

Per Section 5.2, that approval is a distinct act from approving this section. Confirming "Section 6 is approved" completes the Document Map; it does not, by itself, move the version number. If the bump to Version 1.0 is also approved, say so explicitly and it will be applied — front matter and Document Map updated accordingly. Until then, WF-000 stays at Version 0.2: complete, and eligible.

## Dependencies

This section depends on WF-001–006 not yet existing as written documents — it can define the contract they must satisfy once written, but can't yet verify actual compliance against it, since there's nothing written to check. That's a real limitation, not an oversight, of the same kind Section 4 acknowledged about canonical-source accuracy. It also depends on Section 3's registry format and Section 4's synchronization model remaining stable, since inheritance in 6.3 is defined relative to both.

## Acceptance Criteria

1. Every WF-00X document's purpose is stated using only evidence from the original proposal, not invented detail for the documents that weren't elaborated.
2. The governs-but-does-not-duplicate boundary is stated as a specific, checkable rule, not just asserted.
3. Inheritance and override are both itemized explicitly, not summarized vaguely.
4. The worked example traces a real registry entry already in this document, not a hypothetical one.
5. The completion statement audits every prior forward reference individually rather than asserting they're all resolved.
6. The completion statement distinguishes approving this section from approving the Version 1.0 bump, per Section 5.2.
7. No tool, platform, or unconfirmed tech-stack detail is stated as settled fact.

## Verification Checklist

- [x] WF-001, WF-004, WF-005, WF-006 purposes are stated honestly as under-specified where the source material doesn't elaborate them
- [x] Governs-not-duplicates rule is stated as a checkable boundary between artifact and internal content
- [x] Inheritance list and override list are both itemized explicitly
- [x] Worked example traces a real Section 3 registry entry through a real future event
- [x] Every forward reference made in Sections 1–5 is individually accounted for in the completion audit
- [x] Completion statement requests a distinct confirmation for the Version 1.0 bump, not just Section 6 approval
- [x] No tool, platform, or unconfirmed tech-stack detail is stated as settled fact
- [x] Confirmed by Kingsley — Version 1.0 Approved

---

**Section 6 of 6 complete.**

---

---

# Appendix A — Version History

| Version | Date | Summary |
|---------|------|---------|
| 0.1 | 2026-08-04 | Initial draft completed. |
| 0.2 | 2026-08-05 | Governance refinements, synchronization improvements, and amendment process incorporated. |
| 1.0 | 2026-08-05 | All six sections approved. Document released as the governing specification for the Academic & Professional Identity Stack. |