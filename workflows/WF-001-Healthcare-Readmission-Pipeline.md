# WF-001 — Healthcare Readmission Pipeline: Operating Model & Reusable Engineering Workflow

Version: Not yet assigned — versioning begins at 0.1 upon first complete pass through every planned section (WF-000 §5.1)
Status: In Progress — Sections 1–9 approved; Section 10 drafted, pending confirmation
Author: Kingsley Akenu
Architect: Claude
Last Updated: 2026-08-06
Governed By: WF-000 — Academic & Professional Identity Stack (Version 1.0)
Evidentiary Sources: project_summary.md · presentation_nontechnical.md · README_portfolio.md (authoritative); Master-CV.md §TP-001 (cited as drift evidence only)
Registry Cross-Reference: WF-000 §3, Artifacts 5–7 (Healthcare Pipeline — GitHub Repo, Live Dashboard, Live API) — already registered

---

## Document Map

1. Purpose & Problem Statement — **approved**
2. Scope, Definitions & Governance Inheritance — **approved**
3. Evidence Base & Source Reconciliation — **approved**
4. Objectives & Constraints — **approved**
5. System Architecture & Design Rationale — **approved**
6. Engineering Process — Build Sequence & Diagnostic Discipline — **drafted, pending confirmation**
7. Success Metrics, Fairness & Honest Evaluation — **approved**
8. Testing & CI/CD Verification — **drafted, pending confirmation**
9. Deployment & Live Operations — **approved**
10. Governance & Monitoring Integration — **drafted, pending confirmation**
11. Reusable Engineering Patterns — The Operating Model for WF-002–WF-006 — pending
12. Relationship to WF-000 & Compliance Audit — pending

---

# 1. Purpose & Problem Statement

## Purpose

WF-001 establishes the structured record of how the healthcare readmission pipeline was actually built: the objectives it pursued, the constraints that bounded its design, the architecture it settled on and why, the process by which its failures were diagnosed and resolved, the metrics it achieved and precisely what those metrics do and do not certify, and the governance layer that keeps its claims honest. Per WF-000 §6.1, it does this as the first of six planned WF-00X documents, so that the documents which follow it — WF-002 through WF-006 — inherit a working pattern rather than starting from nothing.

Its function is preventive and generative, not archival. A retrospective that only describes what happened would be archival — a historical record with no forward use, no different in kind from the README it's built from. Absent WF-001, two things are true right now, both demonstrated below rather than assumed: the reasoning behind the pipeline's engineering decisions exists nowhere in a form designed to be extracted and reapplied to a different project, and the plain factual record of what was built is already drifting — in a document explicitly designated as this project's canonical source — only weeks after the project's own completion date.

**Evidence 1 — the factual record has already drifted.** Master-CV.md describes itself as the canonical source from which "all resumes, scholarship CVs, portfolio content, LinkedIn updates, and academic applications derive." Its entry for TP-001 (the Healthcare Readmission Prediction Pipeline) lists the technology stack as Python, FastAPI, Docker, GitHub Actions, Azure, MLflow, and DVC. The pipeline that was actually built and deployed is written in R, queries DuckDB with the httpfs extension, trains with tidymodels (XGBoost and glmnet), retrieves clinical guidelines via TF-IDF and generates discharge summaries with ellmer, serves predictions through plumber, and is deployed on Railway and shinyapps.io with object storage on Backblaze B2 — a description independently and consistently corroborated by project_summary.md, presentation_nontechnical.md, and README_portfolio.md, down to matching AUC-ROC and recall figures, matching live URLs, and matching file inventories. Master-CV.md's own "Evidence" list for TP-001 doesn't mention a live dashboard at all, despite the dashboard being a fully deployed, independently documented component of the system. Whichever account was written first, the document positioned as authoritative is the one that no longer describes the system that exists — and this holds regardless of which account eventually proves more accurate in some deeper sense; a mismatch between a designated-canonical source and the system it describes is itself the evidence.

**Evidence 2 — the reasoning was never captured in reusable form to begin with.** The most distinctive piece of engineering judgment in the entire project is the Stage 4 diagnostic sequence: a near-random model (AUC 0.55–0.57) was traced not to a bug in the modeling code but to a data-generation flaw several stages upstream, in Stage 1, and a subsequent suspicious spike in one feature's importance (30.6% of XGBoost's gain, concentrated in a single lab value) was proven to be a coincidental artifact of random-number consumption — not a real signal — by checking whether a simpler model agreed, and finding that it did not. That sequence of reasoning exists today only as prose describing this one incident, on this one project. Nowhere in the current record does "when a feature's importance jumps and a simpler, differently-randomized model doesn't corroborate it, suspect a coincidental artifact of the data pipeline before trusting the signal" exist as a named, portable technique — available on purpose to WF-003's agent-evaluation work, or to any future project, independent of healthcare, independent of XGBoost. The knowledge survived the project. It did not survive extraction.

These are different problems, not the same problem under two names. WF-000's identity-stack drift (§1) is a disagreement between two or more artifacts about a shared fact — it presumes the fact was captured correctly somewhere, and asks only whether every artifact agrees with it. What Evidence 2 demonstrates has no such presumption: even a project with zero disagreeing artifacts can still lose its own reasoning, if none of its artifacts were ever designed to preserve reasoning in reusable form. WF-001 names this second condition so it can be tested for and closed, the same way WF-000 named drift so it could be tested for and closed:

> **Operating Model Loss**: a state in which a completed project's outcome — what was built — remains documented, while the reasoning that produced it — why specific technical choices were made, what constraints bounded them, how failures were diagnosed and resolved, and what "success" was actually defined to mean — exists only implicitly, scattered across artifacts built for other purposes, none of which was designed to be extracted and reapplied to a different project.

Evidence 1 is not itself an instance of Operating Model Loss — it's an instance of the identity-stack drift WF-000 already governs, and Section 3 resolves it formally using WF-000 §4's own Drift Detection Procedure. It's included here because it demonstrates the more general pattern that motivates urgency: if the plain factual layer of this project is already eroding six weeks after completion, the reasoning layer — which was never even captured as cleanly as the facts were — is at greater and more immediate risk.

## Scope

**In scope for this section:** a working definition of Operating Model Loss, formally distinguished from WF-000's identity-stack drift, and the evidence-based case — two independent, checkable examples — for why WF-001 needs to exist now.

**Out of scope for this section** (deferred to later sections):
- The complete enumeration of what this document will capture — objectives, constraints, architecture, process, metrics, testing, deployment, governance — Sections 4–10
- The formal resolution of the Master-CV.md conflict via WF-000 §4's Drift Detection Procedure — Section 3
- New vocabulary this document needs beyond Operating Model Loss (Stage, Diagnostic Round, Locked Decision, Generalizable Pattern, and others) — Section 2
- The extraction of the reusable template itself — Section 11
- Tracking future instances of drift or Operating Model Loss beyond the two cited here — Section 3's evidence base, as it extends over time

**Explicitly out of scope for WF-001 as a whole:**
- Correcting, modifying, or reinterpreting Master-CV.md — a follow-up action for Kingsley outside this workflow, logged as an open item in Section 12
- Re-registering the Healthcare Pipeline's GitHub repository, dashboard, or API as identity artifacts — already registered in WF-000 §3, Artifacts 5–7
- Narrative resume copywriting, portfolio-site content, or any other execution-level rewriting of a public-facing artifact — belongs to the artifact itself

## Outputs

Section 1 establishes the Operating Model Loss definition above. Section 2 formalizes it into the glossary; Section 3 uses it to confirm the Master-CV.md finding classifies as drift rather than Operating Model Loss; Section 11 uses it to confirm the extracted template actually closes the gap it identifies.

## Acceptance Criteria

1. The problem statement is falsifiable against real, current evidence — not a hypothetical — and against two independent examples, not one.
2. Operating Model Loss is defined precisely enough to be distinguishable from WF-000's identity-stack drift, not a restatement of it under a new name.
3. The Master-CV.md finding is presented strictly as evidence; it is not corrected, modified, or reinterpreted anywhere in this section.
4. Nothing in this section promises a capability (e.g., "a reusable template") that a later section doesn't go on to define a mechanism for.
5. A reader with no prior context on this repository can read this section alone and understand why WF-001 needs to exist.

## Verification Checklist

- [x] At least two real, independently-sourced examples are cited and independently checkable against the source artifacts
- [x] Operating Model Loss is defined with an explicit, stated distinction from WF-000's identity-stack drift
- [x] The Master-CV.md finding appears only as cited evidence — no correction, edit, or reinterpretation of Master-CV.md appears anywhere in this section
- [x] Purpose statement makes no unfulfilled promise (the reusable template is named as this document's eventual output, not claimed as delivered here — that is Section 11's job)
- [x] Cross-reference to WF-000 §6.1 is accurate to WF-000 as currently written
- [x] Confirmed by Kingsley before Section 2 begins

---

**Section 1 of 12 complete. Approved 2026-08-06.**

---

# 2. Scope, Definitions & Governance Inheritance

## Purpose

Section 2 does two things Section 1 deliberately deferred: it draws WF-001's document-wide scope boundary, and it supplies the vocabulary the rest of this document needs. Two boundary problems motivate it, mirroring the two that motivated WF-000 §2 for the identity stack. First, without an explicit boundary, "operating model documentation" can expand to include anything loosely related to the pipeline — a live risk for a document whose subject is an already-completed, richly-documented project with no natural stopping point. Second, without a settled vocabulary, later sections would each have to define "pattern," "stage," or "operating model" for themselves, drifting slightly with each use — the same failure mode WF-000 §2 named for "canonical source" and "sync event."

This section also carries the inheritance obligation stated in WF-001's own charter: WF-001 must inherit WF-000's governance principles without duplicating them. Section 2 is where that obligation gets executed — by reference, not by restatement.

Section 1 named the problem: Operating Model Loss, the condition in which a project's reasoning goes uncaptured. This section names what closing that gap actually produces. A completed project generates engineering reasoning — the specific judgment calls, diagnoses, and design choices made along the way, as documented in Sections 4 through 10. Left alone, that reasoning stays bound to the project that produced it. Extracted deliberately — named, stripped of its project-specific detail, and stated in a form that applies elsewhere — it becomes a reusable Engineering Pattern. The set of patterns extracted this way, together with the objectives, architecture, and governance model that produced them, is what this document calls the pipeline's Operating Model: the thing WF-002 through WF-006 actually inherit. Section 11 is where that extraction happens formally; this section only fixes the vocabulary the extraction will use.

## Scope

**In scope for this section:** the document-wide scope boundary for WF-001, and the complete glossary of WF-001-specific vocabulary — including the Operating Model Loss / Engineering Pattern / Operating Model relationship.

**Out of scope for this section** (deferred to later sections):
- The actual extraction of any Engineering Pattern from the pipeline's history — Section 11
- Formal application of WF-000's Drift Detection Procedure to the Master-CV.md finding — Section 3
- The objectives, architecture, process, metrics, testing, deployment, and governance content itself — Sections 4–10

## Outputs

### Document Scope Statement

**In scope:** the healthcare readmission pipeline's objectives, constraints, architecture, engineering process, metrics, testing, deployment, and governance, as evidenced by `project_summary.md`, `presentation_nontechnical.md`, and `README_portfolio.md` — and the Engineering Patterns extracted from that material for reuse by future WF-00X documents.

**Out of scope:** the pipeline's live deployment endpoints as governed objects (WF-000 §3, Artifacts 5–7 already own that); Master-CV.md as a source of fact (Section 1); any future WF-00X document's own architecture, stack, or subject-matter decisions (WF-000 §6.4 reserves these to that document, and WF-001 has no more claim to them than WF-000 does).

**Boundary case, resolved:** if a future WF-00X document reuses an Engineering Pattern extracted here and meaningfully modifies it for its own context, the modification is proposed back to WF-001 — the pattern's canonical home — through the ordinary amendment process, rather than forked silently in the downstream document. WF-001 governs a pattern's canonical definition the way WF-000 governs a canonical source for a fact: a downstream document can extend or specialize a pattern's application, but ownership of the pattern's definition doesn't travel with it. This prevents silent divergence: improvements discovered in later workflows strengthen the shared Engineering Pattern rather than creating competing versions of the same idea.

Applied to specific candidates:

| Candidate | In scope? | Reasoning |
|---|---|---|
| The pipeline's engineering reasoning (Stages 0–9) | Yes | What Sections 4–10 document and Section 11 extracts from |
| The pipeline's live deployment endpoints (URLs, uptime) | No | Already governed by WF-000 §3, Artifacts 5–7 |
| Correcting Master-CV.md's TP-001 entry | No | Kingsley's action, outside this workflow (Section 1) |
| A future WF-002 document's own architecture decisions | No | WF-002's own content, per WF-000 §6.4 |
| An Engineering Pattern extracted here, later modified by a downstream document | Boundary case | Canonical definition stays in WF-001; modifications proposed back via amendment process |

### Glossary of Terms

- **Operating Model Loss** — as defined in Section 1: a state in which a completed project's outcome remains documented while the reasoning that produced it exists only implicitly, scattered across artifacts not designed to preserve it for reuse.
- **Engineering Pattern** — a named, project-independent technique, decision rule, or design choice, extracted from the engineering reasoning behind a specific project and stated in a form intended to apply beyond that project. An Engineering Pattern is the unit Operating Model Loss puts at risk and the unit Section 11 formally extracts. Until it has actually been reused by a second project, its reusability is a design intent, not a confirmed fact — Section 11 states this honestly for each pattern extracted, rather than presenting reusability as already proven. ("Reusable Engineering Pattern" refers to the same term, used where emphasis on reusability is useful.)
- **Operating Model** — the complete, structured account of how a project was run — its objectives, constraints, architecture, engineering process, metrics, testing, deployment, and governance — together with the Engineering Patterns extracted from it. Where an Engineering Pattern is one portable unit, the Operating Model is the aggregate record a downstream WF-00X document actually inherits.
- **Stage** — one of the ten sequential phases (Stage 0 through Stage 9) the healthcare pipeline was built in. Stage 0 covers environment setup; Stages 1 through 9 (referred to in the source material as "the nine stages") cover the substantive pipeline build. Each stage carried its own verification gate before the next began.
- **Diagnostic Round** — one iteration of the detect → investigate → fix → re-verify sequence used in Stage 4 to resolve the model's underperformance, of which there were three. Diagnostic Round is the unit Section 6 examines to extract Engineering Patterns from the pipeline's debugging process specifically, as distinct from its architectural or deployment decisions.
- **Locked Decision** — a governance determination recorded in the pipeline's own `docs/00_locked_decisions.md` — for example, that `approved = TRUE` certifies only the Recall ≥ 0.85 floor, not clinical utility. A Locked Decision is Process Artifact-level content in WF-000's sense unless and until WF-001 or a downstream artifact restates its substance as a public claim, at which point WF-000 §4's Drift Detection applies to that restatement.
- **Governance Table** — one of the eight append-only DuckDB tables (`ingest_metadata`, `feature_registry`, `model_registry`, `fairness_reports`, `rag_chunks`, `rag_index_metadata`, `llm_call_log`, `predictions_audit`) that record the pipeline's own operational history. Like a Locked Decision, a Governance Table is Process Artifact-level content unless a specific figure from it is restated externally.

*A terminology note, not a glossary entry:* this document uses "governance" in two senses that share a word but not a referent. WF-000's governance (its Sections 4–5) concerns identity-artifact consistency — drift, canonical sources, approval authority. The pipeline's own governance (Section 10 below; Locked Decision and Governance Table above) concerns the pipeline's internal audit trail and policy checks — a feature of the system being documented, not of this document's own oversight. Where the distinction matters, later sections say which one they mean explicitly.

### Inherited From WF-000 (By Reference)

The following apply to WF-001 in full, as already defined in WF-000, and are not restated here: the Section 2 glossary (Identity Artifact, Canonical Source, Sync Event, Drift, Process Artifact, Governing Document); the Section 2 scope boundary, under which WF-001 is itself a Process Artifact unless made public; the Section 3 registry format, for any new public artifact WF-001 might cause to be created; the Section 4 synchronization model in full — trigger taxonomy, Drift Detection Procedure, Verification Workflow, and the four workflow roles; and the Section 5 governance model — versioning scheme (WF-001's own version begins at 0.1, per WF-000 §5.1 and §6.3), approval authority (Kingsley, exclusively), and amendment process.

Per WF-000 §6.4, WF-001 cannot override any of the above, cannot leave a new public artifact unregistered, and cannot bypass Kingsley as approver. WF-001 retains full authority over its own internal technical content — the vocabulary, engineering methodology, extracted Engineering Patterns, and the objectives, architecture, process, metrics, testing, deployment, and governance content of Sections 4 through 10 — none of which WF-000 has, or should have, an opinion about.

## Acceptance Criteria

1. Every term used in Sections 1 and 3–12, as currently planned, is defined here with no forward reference to an undefined term.
2. Engineering Pattern and Operating Model are each defined precisely enough to be distinguished from each other and from Operating Model Loss — not three words for the same idea.
3. The Engineering Pattern definition states explicitly that reusability is a design intent until confirmed by actual reuse, rather than asserting it as already proven.
4. Every element inherited from WF-000 is stated by reference, with a specific section citation, not re-derived or restated in different words.
5. The document-wide scope boundary can be applied to a new candidate directly, without further interpretation.

## Verification Checklist

- [x] Glossary covers Operating Model Loss (by reference to Section 1), Engineering Pattern, Operating Model, Stage, Diagnostic Round, Locked Decision, and Governance Table, plus the governance terminology note
- [x] Engineering Pattern and Operating Model are distinguished from each other explicitly, not left to context
- [x] The Engineering Pattern definition does not claim reusability is proven prior to actual reuse
- [x] Every inherited element cites its specific WF-000 section rather than being restated
- [x] Section 11's title and stub, elsewhere in this document, match the extraction framing established here
- [x] Confirmed by Kingsley before Section 3 begins

---

**Section 2 of 12 complete. Approved 2026-08-06.**

---

# 3. Evidence Base & Source Reconciliation

## Purpose

Section 3 does the work Section 1 pointed to and Section 2 supplied the tool for: it applies WF-000 §4's Drift Detection Procedure, in full, to the Master-CV.md finding named in Section 1, and to the three lower-severity findings noted alongside it during the original document-map review. Nothing in Sections 4 through 10 should assert a factual claim whose evidentiary status has not first been established here. This section therefore serves as the evidence gate for the remainder of WF-001.

This is also the first section to put WF-000's own machinery to direct use rather than simply inheriting it by reference. Section 2 stated that WF-001 inherits the Drift Detection Procedure in full; this section is the demonstration that the inheritance is real, not just declared.

## Scope

**In scope for this section:** formal classification — Synced, Drifted, Gap, or explicitly outside what the procedure can resolve — of every evidence-quality finding raised so far in this document, using WF-000 §4's Drift Detection Procedure.

**Out of scope for this section** (deferred to later sections):
- Correcting Master-CV.md, or any other artifact found to be Drifted — Section 12's action item, not this section's
- The substantive content Sections 4–10 will build on top of these now-classified facts

## Outputs

### Source Inventory

| Source | Type | Role |
|---|---|---|
| `project_summary.md` | Internal technical summary, stage-by-stage | Authoritative — the most granular technical detail |
| `presentation_nontechnical.md` | Stakeholder presentation, technical and layman notes per slide | Authoritative — cross-checks `project_summary.md`'s claims against a differently-purposed retelling |
| `README_portfolio.md` | Public GitHub README | Authoritative — the public-facing account, cross-checks the other two |
| `Master-CV.md` §TP-001 | Personal CV entry, self-described as the canonical source other identity artifacts derive from | Not authoritative for technical pipeline facts, per Kingsley's confirmed determination (2026-08-05) — cited only as evidence of drift |

### Applying WF-000 §4's Drift Detection Procedure

**Case 1 — the technology stack**

| Step | Value |
|---|---|
| F | The technology stack used to build and deploy the Healthcare Readmission Pipeline |
| Canonical source | `project_summary.md`, `presentation_nontechnical.md`, and `README_portfolio.md` jointly — R, DuckDB with httpfs, tidymodels (XGBoost, glmnet), ellmer, plumber, Shiny, deployed on Railway and shinyapps.io, storage on Backblaze B2/MinIO |
| Dependent artifact | `Master-CV.md`, §TP-001, "Core Technologies" |
| Dependent claim | Python, FastAPI, Docker, GitHub Actions, Azure, MLflow, DVC |
| Comparison | Dependent claim ≠ canonical value — no overlap between the two lists |
| Classification | **Drifted** |

**Case 2 — the dashboard's existence**

| Step | Value |
|---|---|
| F | Whether a live interactive dashboard exists as a component of the pipeline |
| Canonical source | `README_portfolio.md`'s Live Demo table and `presentation_nontechnical.md` Slide 15 — yes, a five-tab Shiny dashboard deployed to shinyapps.io |
| Dependent artifact | `Master-CV.md`, §TP-001, "Evidence" list |
| Dependent claim | None — the list names GitHub Repository, Live API, Documentation, and Architecture Diagrams; no dashboard entry appears |
| Comparison | No claim exists to compare against the canonical value |
| Classification | **Gap** |

Case 1 and Case 2 are different classifications for a reason, not by accident: Case 1 is an artifact actively asserting something false; Case 2 is an artifact staying silent about something true. Per WF-000 §4, Drift means correcting an active error; Gap means deciding whether the silence needs filling. Both point to the same underlying artifact (`Master-CV.md` §TP-001) and the same next action — logged as a follow-up item in Section 12, not corrected here, per Kingsley's explicit instruction.

### Other Findings, Classified

Two of the remaining findings are straightforward applications of the same procedure:

| Finding | Canonical value | Conflicting claim | Classification |
|---|---|---|---|
| How many governance tables the monitoring script reads | `project_summary.md` Stage 8 and `README_portfolio.md`'s Governance Layer table: 8, both naming all eight directly | `presentation_nontechnical.md` Slide 16: "reads all 7 governance tables" | **Drifted** — two sources against one; "8" is treated as canonical in Section 10 |
| How many phases `presentation_nontechnical.md` organizes its own deck into | The deck's own body: 6 phases (Phase 1 through Phase 6), which do sum to the stated 20 slides | The deck's own header: "20 slides across 5 phases" | **Drifted** — internal to a single document; noted for completeness, doesn't affect any claim WF-001 makes |

The fourth finding — the "71 tests" total not reconciling with the per-file counts shown — doesn't fit this procedure, and it's worth being precise about why rather than forcing a classification onto it. Drift Detection presumes a trustworthy canonical source and checks whether a dependent artifact agrees with it. Here, all three technical documents state the identical per-file breakdown (7 + 8 + 5 + 5 + 5 = 30) alongside the identical "71 total, 0 failures" headline, with no dependent-versus-canonical disagreement to classify — the candidate canonical source is internally self-contradictory on this one point. WF-000 §4's own Dependencies section names exactly this limitation: Drift Detection "only checks consistency with the canonical source, not the canonical source's own accuracy." This case sits outside what the inherited procedure can resolve. Section 8 reports the 71-test headline — the figure repeated identically three times, and the only number with any claim to being load-bearing — and flags the per-category breakdown as unreconciled, rather than fabricating a resolution the source material doesn't support.

### Evidence Hierarchy for the Remainder of WF-001

Unless explicitly noted otherwise, technical claims made in Sections 4 through 12 are derived from the three authoritative technical documents identified above. Any contradiction introduced by another artifact is first evaluated through this reconciliation process before it can affect the Operating Model.

## Acceptance Criteria

1. Every evidence-quality finding raised earlier in this document is formally classified here, not left as informal narrative.
2. The Master-CV.md classifications follow WF-000 §4's exact procedure — the same four-step test, the same table format used in WF-000 §4's own worked example — not a new or looser method.
3. Where a finding doesn't fit the Drift/Gap taxonomy, that is stated explicitly, citing the specific limitation WF-000 §4 already named for itself, rather than forced into the nearest available box.
4. No correction to Master-CV.md, or any other artifact, is made or implied anywhere in this section — classification only.
5. Every classification cites the exact source passages compared, so a reader can verify the result independently.
6. The section establishes itself as the evidence gate for Sections 4 through 12 and states the resulting evidence hierarchy explicitly, so downstream sections can rely on it rather than re-argue source authority.

## Verification Checklist

- [x] All four findings raised earlier in this document are formally addressed in this section
- [x] The technology-stack and dashboard-omission findings are each run through WF-000 §4's exact four-step procedure and presented in its worked-example table format
- [x] The test-count finding is explicitly identified as outside what Drift Detection can resolve, citing WF-000 §4's own stated limitation, rather than forced into a classification
- [x] No correction, edit, or reinterpretation of Master-CV.md appears anywhere in this section
- [x] Every classification is traceable to specific, named source passages
- [x] The evidence gate function and evidence hierarchy are stated explicitly, so Sections 4 through 12 can cite this section rather than revisit source authority
- [x] Confirmed by Kingsley before Section 4 begins

---

**Section 3 of 12 complete. Approved 2026-08-06.**

# 4. Objectives & Constraints

## Purpose

Section 4 opens the documented instance: the first of the seven content dimensions WF-000 §6.1 assigns to WF-001 — objectives, constraints, architecture, success metrics, testing, deployment, governance. It establishes what the pipeline was actually built to achieve, and what bounded how it could be achieved, so that Section 5's architecture reads as a response to stated requirements rather than an arbitrary set of technology choices.

Every claim in this section is sourced under the evidence hierarchy Section 3 established: derived from the three authoritative technical documents, with no re-litigation of source authority required. Where a claim is inferred rather than directly stated, that distinction is made explicit rather than blurred — this section contains two such cases, handled differently depending on how strong the underlying evidence is.

## Scope

**In scope for this section:** the pipeline's objectives — both the stated clinical problem and the demonstrated portfolio purpose — and the real, evidenced constraints that bounded its design.

**Out of scope for this section** (deferred to later sections):
- The architectural response to these objectives and constraints — Section 5
- The engineering process used to work within these constraints — Section 6
- Metrics and what they certify — Section 7

## Outputs

### Objectives

The pipeline's objectives operate at two levels, both directly evidenced rather than read from a single framing.

**Stated objective — the clinical problem the system addresses.** Flag patients at high risk of 30-day hospital readmission at the point of discharge, explain each flag with the factors that drove it, retrieve the clinical guidelines relevant to that patient's conditions, and generate a cited discharge recommendation. README_portfolio.md frames the underlying cost as a metric explicitly monitored by CMS and insurance providers as a quality indicator, priced at $15,000–$20,000 per unnecessary readmission.

**Actual objective — what the project is built to demonstrate.** The system is explicitly not a clinical product: "FOR PORTFOLIO DEMONSTRATION ONLY — NOT FOR CLINICAL USE" appears as a standing banner in README_portfolio.md and, per project_summary.md, is "clearly labeled in the API's disclaimer on every response." project_summary.md states the real objective directly, under "What This Demonstrates": the pipeline exists to show, for a data science portfolio, a complete ML lifecycle — data engineering, governance, the API, monitoring, CI/CD, and testing, not just the modeling step — with each stage built in sequence with real verification before moving on. The specific skills the source material names as the actual target of the exercise: production R engineering, SQL-first data engineering at scale, responsible ML practice, RAG system design without new packages, testable REST API design, CI/CD with R, and Windows-specific DuckDB debugging.

**A qualifying objective, not merely a constraint: honest disclosure.** project_summary.md frames the decision to report a modest AUC-ROC (0.566) with full root-cause explanation, rather than tuning toward a more impressive number, as "itself a technical and ethical choice worth noting." presentation_nontechnical.md devotes its closing slide to naming this as the project's operating philosophy — KAIZEN (改善) — stating plainly that the word "honest" is "load-bearing." This is classified here as an objective rather than a passive constraint because the source material frames it as something actively chosen and pursued, not a limitation the project merely happened to operate under.

### Constraints

| Constraint | Evidence | Effect on the Pipeline |
|---|---|---|
| Real source data limited to 100 patients (MIMIC-IV MEDS demo); the full ~50,000-patient MIMIC-IV requires PhysioNet credentialing not obtained in this phase | project_summary.md, "What Would Come Next"; presentation_nontechnical.md, Slide 3 and Slide 10 speaker notes | Named directly as the root cause of the model's modest AUC-ROC (0.566) — a signal ceiling, not a modeling defect |
| MIMIC-IV de-identifies by shifting each patient's dates by a random, patient-specific offset | presentation_nontechnical.md, Slide 7 speaker notes | Made a temporal train/test split methodologically meaningless; forced a patient-level split instead |
| The 9-million-row lab measurement table is too large to load into R memory | project_summary.md, Stage 3; presentation_nontechnical.md, Slide 6 speaker notes | Forced a SQL-first architecture — all heavy aggregation computed in DuckDB, never loaded into R |
| Vectorized measurement resampling would explode to roughly 121 million rows without a cap | presentation_nontechnical.md, Slide 4 speaker notes | Capped at 300 labs per visit during synthesis |
| A Windows-specific DuckDB file-locking problem appeared during rapid sequential test execution | project_summary.md, Stage 9; README_portfolio.md, Testing section | Forced a connection-singleton pattern in the test setup |
| A Windows-specific XGBoost `predcontrib` alignment bug (`array_interface.h:422`) | presentation_nontechnical.md, Slide 11 speaker notes | Bypassed with a pure-R explainability approach instead of the native XGBoost path |
| A seven-calendar-day build window — Stage 0 dated 2026-06-19 through Stage 9 dated 2026-06-25, per the dated stage notes | project_summary.md, File Inventory (`notes/` listing) | Not stated as a constraint directly; inferred from the dated notes themselves. Stages 1–3 share a single date (6/20) and Stages 6–7 share another (6/23) |

Team size and resourcing are not addressed by any of the three authoritative technical documents. All three show a single, consistent byline — Kingsley Akenu (@Kayterthesly — KAIZEN 改善) — with no team, collaborator, or division-of-labor reference anywhere in the source material. That is evidence of an unaddressed topic, not evidence of a specific team size; this document notes the gap rather than asserting a headcount the source material doesn't state.

## Acceptance Criteria

1. Every stated objective, constraint, and evidentiary inference traces to a specific, named passage in one or more of the three authoritative technical documents, with inferred conclusions explicitly identified as such.
2. The pipeline's stated (clinical) objective and its actual (portfolio/demonstration) objective are presented as explicitly distinct, not blended into one description.
3. Honest disclosure is classified and justified as an objective rather than a passive constraint, with the specific evidence supporting that classification.
4. Where a candidate constraint (team size and resourcing) is not directly evidenced, that gap is stated explicitly rather than filled with an inference presented as fact.
5. The one constraint presented as an inference from evidence (the seven-day build window) is clearly distinguished from the constraints stated directly in the source material.

## Verification Checklist

- [x] Every objective and constraint cites a specific source document and location
- [x] Stated (clinical) objective and actual (portfolio) objective are presented as explicitly distinct, not merged
- [x] Honest disclosure's classification as an objective, not a constraint, is justified with direct evidence
- [x] The team-size/resourcing gap is stated as an unaddressed topic, not asserted as a fact
- [x] The seven-day build-window finding is labeled as an inference from dated files, distinct from the directly-stated constraints
- [x] Confirmed by Kingsley before Section 5 begins

---

**Section 4 of 12 complete. Approved 2026-08-06.**

---

# 5. System Architecture & Design Rationale

## Purpose

Section 5 answers the question Section 4 leaves open: given those objectives and those constraints, why was the pipeline built this particular way? It is deliberately organized around decisions and their rationale, not around a tech-stack inventory — a list of tools says what was used; this section explains what problem each major structural choice solved, and, where a real trade-off existed, what was given up to solve it.

Every design decision below traces to at least one objective or constraint named in Section 4, cited explicitly rather than assumed. Not every trace carries equal weight — some choices were effectively forced by a constraint (an invalidated alternative, not merely a costlier one), while others were genuine judgment calls between viable options. This section marks that difference each time it applies, rather than labeling every choice a "trade-off" when some were not.

Governance, testing/CI-CD, and deployment mechanics each have their own dedicated section later in this document (10, 8, and 9 respectively). This section treats them only at the level needed to explain the overall shape of the system; the operational depth belongs there, not here.

## Scope

**In scope for this section:** the design rationale behind the pipeline's major architectural decisions, each traced to a specific Section 4 objective or constraint, with design decisions explicitly distinguished from implementation detail and genuine trade-offs distinguished from forced necessities.

**Out of scope for this section** (deferred to later sections):
- The diagnostic process that led to specific corrections (e.g., the Stage 1 v2 severity-timing fix) — Section 6
- The full governance layer (all 8 tables, locked decisions, policy checks) — Section 10
- The full testing and CI/CD detail — Section 8
- Deployment specifics (live providers, the actual storage-provider swap execution) — Section 9

## Outputs

### The Five-Layer Architecture

README_portfolio.md's own architecture diagram names five layers: a data layer (object storage plus DuckDB), an inference layer (the REST API), a presentation layer (the dashboard), a governance layer (the eight audit tables), and a CI/CD layer (the GitHub Actions pipeline). This section uses that same five-layer shape, since it is the system's own documented self-description rather than a categorization imposed from outside.

Three of the five get only summary treatment here because a dedicated section already covers their operational depth: governance in Section 10, CI/CD in Section 8, and the deployment specifics of where the data and inference layers actually run in Section 9. What follows is the full design rationale for the data and inference layers, and a brief note on why governance and presentation are shaped the way they are.

### Design Decision 1 — SQL-First Computation Over a Swappable Storage Backend

**Traces to:** Section 4's constraint that the nine-million-row lab measurement table is too large to load into R memory.

**What was built:** All heavy aggregation — the computation behind the pipeline's 81 features — runs as SQL directly against Parquet files via DuckDB's httpfs extension, which registers cloud object storage as virtual SQL tables. The measurement data itself never enters R as an in-memory data frame; only the aggregated results do.

**Why:** The constraint made an in-memory R workflow — the more typical R data-science pattern — infeasible at this data volume. Pushing computation into the database engine avoids the memory ceiling entirely, rather than working around it with chunking or sampling.

**Trade-off:** this is not a cost-free choice. SQL-first computation is less interactive than an in-memory data frame — exploratory analysis requires writing SQL rather than piping through familiar R verbs, and it demands SQL fluency alongside R fluency. The pipeline accepts this cost in exchange for a data volume ceiling that would otherwise make the nine-million-row table unusable in R at all.

**A downstream consequence, not an independently-traced decision:** because computation already happens through SQL against Parquet-by-URL rather than through R code that assumes a specific file location or vendor SDK, the storage backend itself becomes a configuration detail rather than an architectural assumption — `global_config.R` is described as provider-agnostic for exactly this reason. This is a benefit of Design Decision 1, not a second decision independently justified by its own Section 4 trace; the actual provider swap (MinIO to Backblaze B2, the six environment variables) is deployment mechanics, covered in Section 9.

### Design Decision 2 — Synthesize Rather Than Train Directly on the 100-Patient Source

**Traces to:** Section 4's constraint that real source data is limited to 100 patients, with credentialed access to the full MIMIC-IV not obtained in this phase.

**What was built:** The 100 real patients are reshaped into four canonical tables (person, visit, condition, measurement) and synthesized up to 15,000 patients using `synthpop`'s CART algorithm for demographic and visit attributes, bootstrap resampling from real inter-admission gaps for visit timing, empirical frequency resampling for diagnosis codes, and capped vectorized resampling for lab measurements.

**Why:** 100 patients is too small a population to learn patterns that only emerge at scale — the source material's own framing is that a model trained on 100 patients is comparable to a doctor who has only ever seen 100 patients. Synthesis responds to that specific limitation; it is not a general preference for synthetic data.

**The governing design principle:** preserve the real data's statistical properties rather than invent new ones. The synthesized population's 20.06% readmission rate matches the real source's 20.1% — evidence that the synthesis extrapolated mathematically from what the 100 real patients actually showed, rather than fabricating a plausible-looking number.

**The same fact as a Section 4 constraint, seen from the other side:** synthetic data built from a 100-patient source cannot exceed the information that source contains. This is the same signal ceiling Section 4 names as the direct cause of the model's modest AUC-ROC — not repeated in full here, since Section 4 already covers it, but the two are the same underlying fact viewed from opposite ends: there, a limit on what the model could learn; here, a limit on what the architecture could responsibly synthesize.

**Deferred, not detailed here:** the specific mechanism by which visit timing and diagnosis severity were later linked (the shared latent "is_severe" variable) was a correction made during the build process, not part of the original design — it belongs to Section 6's diagnostic narrative, not to this section's account of the initial architecture.

### Design Decision 3 — Preventing Data Leakage Through Split Design

**Traces to:** Section 4's constraint that MIMIC-IV de-identifies by shifting each patient's dates by a random, patient-specific offset.

**What was built:** two related choices, both aimed at the same failure mode — a model that appears to perform well because it was tested on information it shouldn't have had. First, the train/test split is performed at the patient level (`rsample::group_initial_split()` on `subject_id`), verified by checking for zero patient-ID overlap between the two sets — not at the record level or by date. Second, `is_deceased` is explicitly excluded as a predictor.

**Why:** date-shifting is applied independently per patient, so one patient's shifted dates are not comparable to another's — a temporal split (train on earlier dates, test on later ones) would compare dates that don't correspond to the same calendar time across patients, making it methodologically meaningless rather than merely suboptimal. `is_deceased` is excluded because death is a future fact relative to any earlier hospital visit: a discharging clinician cannot know at discharge whether a patient will die at some later point, so a model trained to use that information would be learning from data no real deployment could ever supply at prediction time.

**Not a trade-off — a forced choice.** Unlike Design Decision 5 below, this is not a case of weighing two viable options against each other. The alternative — a temporal split — was invalidated by the de-identification method itself, not merely less desirable. This document distinguishes the two because treating every design choice as a trade-off would understate how directly some decisions follow from a constraint that leaves no real alternative.

**Implementation detail, not itself the design decision:** the specific split ratio (80% train — 12,000 patients; 20% test — 3,000 patients) is a parameter choice within this design, not a separate structural decision.

### Design Decision 4 — Testable-by-Design API Architecture

**Traces to:** Section 4's actual objective, which names "testable REST API design" directly among the skills the project exists to demonstrate.

**What was built:** the API's business logic lives in plain R functions (the `*_core()` pattern); the Plumber decorators that expose them as HTTP routes are thin wrappers around those functions.

**Why:** if request-handling logic and business logic are the same code, testing that logic requires running an HTTP server and making real requests against it — slower, harder to isolate, and harder to run as part of an automated suite. Separating the two lets the API-related tests (part of the 71-test suite) call the underlying functions directly, without a server in the loop.

**Trade-off:** the separation adds a layer of indirection that a smaller, untested prototype wouldn't need — every route has a corresponding core function to maintain in parallel. The pipeline accepts this added structure in exchange for an API whose correctness is verified automatically rather than only by manual inspection.

**Implementation detail:** loading the model once at process startup, rather than per-request, is a performance choice within this architecture, not the design decision itself.

### Design Decision 5 — Interpretable Retrieval Over Semantic Depth

**Traces to:** Section 4's actual objective, which names "RAG system design from scratch without new packages" directly, and the honest-disclosure objective, since an auditable retrieval method is what makes a discharge recommendation's sourcing checkable rather than merely asserted.

**What was built:** clinical-guideline retrieval uses a hybrid of TF-IDF cosine similarity (40%), keyword density (30%), and ICD-code metadata overlap (30%), implemented in base R with no additional retrieval package. A template-based fallback generates a discharge summary without calling the Gemini API at all, so the pipeline continues to function if that external service is unavailable.

**Why:** TF-IDF is dependency-free and its results are directly traceable to term overlap — a clinician or auditor can see exactly why a given guideline chunk was retrieved. This serves the same governance and honesty commitments Section 4 documents elsewhere: a retrieval mechanism that can be inspected is one whose claims can actually be checked, not just trusted.

**The trade-off is stated directly in the source material, not inferred here.** The project's own roadmap section names the limitation explicitly: because TF-IDF works by matching shared terms, a clinical guideline that describes the same underlying concept in different wording than the patient's diagnosis codes could be missed entirely — a failure mode a semantic embedding model wouldn't have, at the cost of adding a dependency and losing some of TF-IDF's transparency. The design decision knowingly accepted that risk in exchange for interpretability and zero new dependencies, with the upgrade path already identified for a future phase.

### Governance as a First-Class Layer, Not an Add-On

**Traces to:** Section 4's actual objective, which names "governance, fairness, honest metrics, audit trails" among the skills demonstrated, and the honest-disclosure objective directly — honesty that can't be checked is only an assertion.

README_portfolio.md's architecture diagram places a governance layer alongside the data, inference, and presentation layers, not beneath or after them — eight append-only DuckDB tables record every ingestion, feature definition, model training run, fairness stratification, RAG index build, LLM call, and prediction, with patient identifiers and raw inputs stored only as hashes. Placing governance at this level, architecturally equal to the layers that do the pipeline's actual work, is itself the design decision: an audit trail bolted on after the fact would sit outside the system's normal data flow and could be skipped; a governance write built into each stage's own code cannot be.

One design implication worth naming, though the source material doesn't frame it in these terms directly: hashing protects patient privacy in the audit trail, but by the same construction it also means the audit trail cannot later be used to inspect what the actual input values were — only that a call happened, with what shape of input. That is a reasonable consequence of the hashing choice, noted here as an implication of the design rather than a claim the source material states outright.

The eight tables individually, the locked-decisions document, and the six-policy check engine are covered in full in Section 10; this section establishes only why governance sits where it does in the architecture.

### The Presentation Layer, Briefly

The dashboard (Shiny, shinydashboard, Plotly, and DT across five reactive tabs) is architecturally the pipeline's fourth layer, and its design is comparatively simple relative to the decisions above: it reads a pre-computed governance snapshot for static content and makes live calls to the Railway API for patient-specific predictions, rather than connecting to DuckDB directly. This traces to the same actual objective as the rest of the system — a "complete ML lifecycle" demonstration needs a way for a non-technical audience to see the system work, which is the same dual-audience framing presentation_nontechnical.md itself is built around. The specific hosting mechanics of this layer belong to Section 9.

### Traceability Summary

| Design Decision | Traces to (Section 4) |
|---|---|
| SQL-first computation; swappable storage as a consequence | Constraint: nine-million-row memory limit |
| Synthesize rather than train directly on 100 patients | Constraint: 100-patient source limit |
| Patient-level split; exclude `is_deceased` | Constraint: MIMIC-IV date-shifting |
| Testable-by-design API (`*_core()` pattern) | Actual objective: "testable REST API design" |
| TF-IDF hybrid retrieval, no new packages | Actual objective: "RAG system design... without new packages"; honest-disclosure objective |
| Governance as a first-class architectural layer | Actual objective: "governance... audit trails"; honest-disclosure objective |
| Presentation layer's live-plus-static hybrid | Actual objective: complete-lifecycle demonstration |

## Acceptance Criteria

1. Every design decision discussed traces explicitly to at least one Section 4 objective or constraint, cited by name.
2. Each decision states why it was made, not only what was built — a bare tech-stack list would fail this criterion even if technically accurate.
3. Design decisions are explicitly distinguished from implementation details throughout, not blended.
4. Where a genuine trade-off exists, it is stated plainly; where a choice was effectively forced by an invalidated alternative rather than a costlier one, that distinction is made rather than calling it a trade-off by default.
5. No technology, package, or architectural claim appears that isn't supported by `project_summary.md`, `presentation_nontechnical.md`, or `README_portfolio.md`.
6. Content that belongs to a later section (governance depth, testing/CI-CD depth, deployment mechanics) is deferred explicitly rather than duplicated here.

## Verification Checklist

- [x] A traceability table maps every design decision to its Section 4 origin
- [x] Each Design Decision subsection states rationale (why), not just composition (what)
- [x] Implementation-detail callouts are marked distinctly from design-decision rationale throughout
- [x] At least one genuine, source-stated trade-off (TF-IDF vs. embeddings) and at least one forced-not-traded choice (patient-level split) are both present and explicitly distinguished
- [x] No technology or claim appears without a citation to one of the three authoritative documents
- [x] Governance, testing/CI-CD, and deployment depth are explicitly deferred rather than duplicated
- [x] Confirmed by Kingsley before Section 6 begins

---

**Section 5 of 12 complete. Approved 2026-08-06.**

---

# 6. Engineering Process — Build Sequence & Diagnostic Discipline

## Purpose

Section 6 documents the engineering process — not what exists at the end (Section 5's job) but how the pipeline was actually built and how its failures were actually diagnosed. It exists because process, not architecture, is the part of this project most likely to transfer intact to a domain with a completely different technology stack: a future WF-00X project may not use R, DuckDB, or XGBoost, but the discipline of gating each stage before the next begins, and of investigating a symptom past the boundary of the stage where it appeared, has nothing to do with any of those specific choices.

This section deliberately does not retell `project_summary.md`'s stage-by-stage summary. Where the source material states what a stage produced, this section asks why that stage had to come where it did and what its gate actually checked; where the source material narrates the Stage 4 diagnostic rounds as a sequence of events, this section treats each round as a specific reasoning method and asks what makes that method generalizable. The result is analysis built on the same source material, not a restatement of it.

## Scope

**In scope for this section:** the Stage 0–9 build sequence and the dependency logic between stages; the verification mechanism specific to each stage; a detailed treatment of Stage 4's three diagnostic rounds as the project's central engineering case study; and a locator list of candidate material for Section 11's formal pattern extraction.

**Out of scope for this section** (deferred to later sections):
- The formal naming and structured extraction of Engineering Patterns — Section 11
- The metrics Stage 4 and Stage 5 ultimately produced, and what they certify — Section 7
- The full test suite (Stage 9's 71 tests) in operational detail — Section 8

## Outputs

### The Build Sequence: Stage 0 Through Stage 9

| Stage | What Was Built | Why It Had to Follow the Prior Stage | Verification Mechanism |
|---|---|---|---|
| 0 — Workspace | Reproducible environment: renv lock (189 packages), `.Renviron` secrets, provider-agnostic storage config | First by necessity — every later stage depends on a working, reproducible environment existing | Reproducibility itself (the renv lock pins exact versions); no separate documented gate |
| 1 — Data | 100 real patients synthesized to 15,000 via `synthpop` | Needs Stage 0's storage connection to read and write data at all | Statistical fidelity check: the synthesized readmission rate (20.06%) matched the real source's rate (20.1%) |
| 2 — Ingestion | Canonical casting, referential integrity, PHI gate | Feature engineering (Stage 3) must not be built on unvalidated data | Explicit ingest gate: type-casting, null checks, referential integrity, and a PHI/ENV_MODE check that rejects real patient data outside production |
| 3 — Features | 81 features computed in SQL against nine million rows | Models (Stage 4) need a feature set to train on | Automated zero-leakage-column check |
| 4 — Modeling | Two models (glmnet, XGBoost), three diagnostic rounds | Explainability and fairness analysis (Stage 5) are properties of a specific trained model's predictions — nothing to explain or audit before a model exists | Recall ≥ 0.85 floor for the `approved` flag; a self-imposed limit of three test-set-adaptive tuning rounds |
| 5 — Explainability & Fairness | Permutation importance; fairness stratification by subgroup | Needs Stage 4's trained model and predictions to analyze | Quantified fairness-flagging threshold: a subgroup recall range exceeding 15 percentage points, with n ≥ 30, triggers a flag |
| 6 — RAG | Guideline retrieval and discharge-summary generation | *(Process sequencing, not a hard dependency)* — follows Stage 5 in the build sequence, though the technical dependency is narrower — see note below | Section 12 governance contract structure (`summary_text`, `citations`, `retrieval_debug`, `trace_id`, `model_version`, `index_version`) on every call |
| 7 — API | Four REST endpoints exposing predict, explain, and RAG | Genuinely depends on Stages 4, 5, and 6 all existing — its endpoints are thin wrappers around exactly those three capabilities | Operational invariant: every response carries a `trace_id`; every prediction writes an audit row |
| 8 — Observability | Monitoring script, six-policy check, CI/CD pipeline | Needs Stage 7's API running to have `predictions_audit` rows worth monitoring, and needs an assembled system to run CI against | Explicit six-policy check enforcing governance invariants |
| 9 — Testing | 71 tests (55 unit, 16 integration), zero failures | Integration tests exercise the assembled system end-to-end — they could not be finalized until the components they integrate (model, API, governance tables) all existed | The stage is itself the gate: the last checkpoint in the nine-step CI/CD pipeline before deployment |

Most of these transitions are hard technical dependencies — Stage 3 cannot compute features on data Stage 2 hasn't validated; Stage 5 cannot explain a model Stage 4 hasn't trained. Two transitions are worth being precise about, because the executed order is not, on the evidence available, a strict technical requirement. Stage 6 (RAG) draws on a patient's diagnosis codes (available since Stage 2) and knowledge of which patients are high-risk (available since Stage 4); nothing in the source material indicates it consumes Stage 5's explainability or fairness output specifically. Its position after Stage 5 looks like a reporting sequence — audit and understand the model fully before building the layer that acts on its output — rather than a dependency the RAG code itself enforces. Separately, Stage 9 is the last stage, but that does not mean testing began only at the end: the source material documents only when the complete, 71-test suite was finalized, not when individual tests were first written. What is directly evidenced is narrower and still real — the 16 integration tests exercise the assembled system end-to-end, from request to audit log, and could not have been finalized before Stage 8's API and governance tables existed to integrate against.

### Stage 4 as Case Study: The Three Diagnostic Rounds

Stage 4 is treated here as the central case study for a specific reason: it is where the source material documents not just a decision, but an iterated, self-correcting investigation — three rounds, each with a different failure mode, a different diagnostic method, and a different kind of fix. It is not the only stage where something went wrong and was fixed; `project_summary.md` counts eight documented bugs across all nine stages. It is chosen here because its three rounds are the most fully diagnosed and the most analytically distinct from one another, which makes the reasoning inside each round easiest to separate out and hardest to lose in a chronological retelling.

**Round 1 — a data-handling bug.** *Symptom:* ROSE, the class-balancing step, crashed. *Investigation:* the crash traced to `is_deceased` — it had been removed from the model's predictor role via `update_role()`, a recipe-level instruction that tells `tidymodels` to ignore a column during modeling, but the column itself was still physically present in the data frame ROSE received. ROSE rejected the character column outright. *Root cause:* removing a column's modeling role is not the same operation as removing the column, and ROSE's requirements exposed that distinction where `tidymodels`' own modeling functions would have tolerated it. *Fix:* an explicit `select(-is_deceased)` before the ROSE step. *Nature of this round:* self-contained — the symptom and the cause were in the same place, and no re-diagnosis across stages was needed.

**Round 2 — a root cause two stages upstream.** *Symptom:* AUC of 0.55 to 0.57 — barely above the 0.50 coin-flip baseline — with `n_prior_admissions` dominating feature importance and nothing else contributing meaningfully. *Investigation:* the question this round asked was not "how do I tune the model to perform better" but "why is there nothing here for the model to learn." The answer required looking backward past Stage 4's own code, past Stage 3's feature engineering, to Stage 1's data synthesis. *Root cause:* the original synthesis (Stage 1 v1) generated a patient's clinical severity and their readmission timing independently of each other — two separate random processes with no shared logic connecting them, so no relationship existed for the model to find. *Fix:* Stage 1 was revised (v2) to add a shared latent variable — `is_severe` — that influences both how quickly a patient returns and how severe their diagnoses are, grounded in published 30-day readmission literature rather than invented outright, and explicitly dropped before the data reached the model, so the model itself never saw it directly. *Re-verification:* retraining after the Stage 1 v2 fix moved AUC away from the near-random baseline toward the 0.566 figure ultimately reported, confirming the fix addressed a real defect rather than a coincidental one. *Nature of this round:* the defect and its symptom were in different stages entirely; diagnosing it required treating an upstream, already-completed stage as a legitimate suspect, not only inspecting the stage where the symptom appeared.

**Round 3 — distinguishing signal from artifact by comparing models.** *Symptom:* after the Stage 1 v2 fix, one specific feature — `lab_224168_min`, a single lab value's minimum — jumped to 30.6% of XGBoost's total gain, a concentration of importance on one feature that hadn't appeared before. *Investigation:* the same feature's coefficient in glmnet, the simpler linear model trained on the same data, was near zero. That asymmetry was the diagnostic signal: a real clinical relationship strong enough to account for nearly a third of XGBoost's decision-making should leave some trace in a linear model too, even a small one. Its near-total absence from glmnet while dominating XGBoost pointed to something XGBoost's flexibility could fit that a linear model's structure could not — the signature of an idiosyncratic pattern rather than a genuine relationship. *Root cause:* a coincidental artifact of `rbinom()`'s random-number consumption during the Stage 1 v2 change, which reshuffled which lab values ended up associated with which visits in a way that happened to correlate, by chance, with a specific narrow value range. *Fix:* XGBoost was regularized, reducing its capacity to fit small, idiosyncratic patterns. *Re-verification:* AUC held at 0.566 after regularization — the same figure as before the fix, which is itself the confirming evidence: if the spike had been contributing real predictive value, regularizing it away should have cost something. It didn't, which is what confirmed the spike was noise rather than signal. *Nature of this round:* the diagnostic method itself — comparing a flexible model's behavior against a simpler model's behavior on the same feature — is a technique, not a one-off observation, and it generalizes to any situation where a complex model can be suspected of over-fitting a pattern a simpler model doesn't corroborate.

**Across the three rounds, the diagnostic discipline escalates.** Round 1 is a local bug, fixed where it was found. Round 2 required extending the search past the stage where the symptom appeared to the stage that actually caused it — treating "is my upstream data constructed correctly" as a legitimate diagnostic question, not only "is my model code correct." Round 3 developed a specific, repeatable technique — cross-model comparison as a test for spurious feature importance — that has nothing to do with healthcare data specifically and everything to do with how flexible and simple models behave differently in the presence of noise. This progression, from local fix to cross-stage root cause to a named, transferable diagnostic technique, is the primary material Section 11 draws on.

One further discipline runs underneath all three rounds and is documented explicitly rather than left implicit: each round used the test set's own AUC as a signal for whether to change the upstream pipeline, which the source material itself names as a mild form of test-set-adaptive tuning — a real methodological cost, even if a smaller one than classic overfitting. Iteration was stopped at three rounds for exactly this reason, and the decision to stop, along with the reason for stopping, is disclosed rather than omitted. This is the same honest-disclosure objective documented in Section 4, demonstrated here as a specific, applied engineering habit rather than a general value statement: knowing when further iteration would weaken methodological confidence is treated as part of the discipline, not a limitation to gloss over.

### Candidate Material for Section 11

This section documents reasoning; it does not extract patterns from it — that formal step belongs to Section 11, per the Section 2 definition of Engineering Pattern. What follows is a locator, not an extraction: the specific pieces of reasoning above that look like pattern candidates, so Section 11 does not need to re-mine this section from scratch.

- The cross-model comparison technique from Diagnostic Round 3 — suspect a feature a flexible model trusts but a simpler model doesn't corroborate.
- The root-cause discipline from Diagnostic Round 2 — when a symptom appears in one stage, treat every upstream stage as a legitimate suspect, not only the stage where the symptom surfaced.
- The staged-build-with-a-gate-before-proceeding structure documented above, and the observation that the gate's form varies deliberately by what's being verified — a statistical check, a schema check, a metric threshold, a fairness threshold, a policy check — rather than one generic gate reused everywhere.
- The self-imposed limit on test-set-adaptive tuning (three rounds, stopped and disclosed) as a specific, applied instance of the honest-disclosure objective.

Whether each of these becomes a named Engineering Pattern, and in what form, is decided in Section 11 — this list only prevents that section from starting empty.

## Acceptance Criteria

1. The build sequence is explained by dependency logic, not merely listed in order — including the two transitions (Stage 5→6, Stage 8→9) where the executed order is not, on the evidence, a strict technical requirement.
2. Every stage's verification mechanism is named precisely from the source material, and stages without an explicit documented gate are identified as such rather than assigned an invented one.
3. All three Diagnostic Rounds are documented with symptom, investigation, root cause, fix, and re-verification for each, not summarized as a single narrative.
4. The escalation across the three rounds — local bug, cross-stage root cause, generalizable technique — is stated explicitly, not left for the reader to notice.
5. The test-set-adaptive-tuning disclosure is connected explicitly back to Section 4's honest-disclosure objective.
6. Candidate material for Section 11 is flagged explicitly, without performing Section 11's own formal extraction work here.
7. No claim in this section restates `project_summary.md`'s own language closely enough to be a paraphrase in name only — every claim is analyzed, not merely reformatted.

## Verification Checklist

- [x] The Stage 0–9 sequence is presented with dependency reasoning for each transition, not just an ordered list
- [x] The Stage 5→6 and Stage 8→9 transitions are explicitly flagged as not strictly technically forced, distinct from the transitions that are
- [x] Every stage's verification mechanism is named from the source material; stages without an explicit documented gate (0, 6, 7) are identified honestly rather than assigned an invented one
- [x] All three Diagnostic Rounds follow the same five-part structure — symptom, investigation, root cause, fix, re-verification
- [x] The escalation pattern across the three rounds is named explicitly
- [x] The test-set-adaptive-tuning disclosure is cross-referenced to Section 4's honest-disclosure objective
- [x] A candidate-material list for Section 11 is present and explicitly deferential to Section 11's own formal extraction step
- [x] Confirmed by Kingsley before Section 7 begins

---

**Section 6 of 12 complete. Approved 2026-08-06.**

---

# 7. Success Metrics, Fairness & Honest Evaluation

## Purpose

Section 7 documents the project's outcomes — but treats "outcomes" as four distinct things that are easy to collapse into one and shouldn't be: how well the model performs, whether it clears the governance approval gate, whether it treats population subgroups equitably, and whether its own limitations were disclosed honestly. Collapsing these into a single impression — "the model is good," or "the model is approved" — is exactly the kind of claim inflation Section 1's Operating Model Loss and WF-000's identity-stack drift both exist to guard against; a resume bullet reading only "model approved" would be technically true and substantively misleading. This section keeps the four apart so nothing built on top of it can blur them back together.

Each category is examined for what it certifies and what it does not — the same discipline Section 5 applied to trade-offs versus forced choices, applied here to metrics versus claims. Where the source material states a fact plainly, that is marked as a fact; where it offers an explanation the evidence supports but doesn't prove, that is marked as an inference; where it names its own blind spot, that limitation is preserved rather than smoothed over.

## Scope

**In scope for this section:** the pipeline's reported metrics, its governance approval status, its fairness evaluation, and the honest-disclosure practices around all three — each examined for what it certifies and what it does not, with facts, inferences, and limitations explicitly distinguished.

**Out of scope for this section** (deferred to later sections):
- The diagnostic process that produced these metrics — Section 6 (already covered)
- The test suite that verifies the system's software correctness, as distinct from its statistical performance — Section 8
- The formal extraction of Engineering Patterns from the reasoning below — Section 11

## Outputs

### Category 1 — Model Performance

| Metric | Value | Certifies | Does Not Certify |
|---|---|---|---|
| Recall | 0.885 | Catches 88.5% of patients who will actually be readmitted | That flagged patients are likely to actually be readmitted — see Precision |
| Precision | 0.212 | Of all patients flagged, 21.2% are correctly flagged | That the model is broadly inaccurate — precision must be read against the roughly 20% base rate and the screening, not final-decision, use case |
| AUC-ROC | 0.566 | Discriminative ability modestly above the 0.500 random baseline, across all thresholds | Clinical utility on its own — Section 4 already establishes the 100-patient signal ceiling as the direct cause of this figure |
| PR-AUC | 0.244 (vs. 0.203 base rate) | The model exceeds a trivial base-rate baseline across the precision-recall trade-off, not only at one threshold | That 0.244 represents strong absolute performance — it is a modest margin above a modest baseline |

### Category 2 — Governance Approval

| What `approved = TRUE` Certifies | What It Does Not Certify |
|---|---|
| Recall ≥ 0.85 was met for that specific model version | Clinical utility, precision, AUC-ROC quality, or fairness — none of these are checked by the approval flag itself |

The approval mechanism has a documented weakness, disclosed rather than hidden: a gate that checks only recall can be satisfied trivially by lowering the classification threshold until nearly every patient is flagged, since a model that flags everyone catches 100% of true readmissions by construction — recall approaching 1.0 at the cost of precision approaching the base rate, and the tool becoming useless as a screening instrument even while technically "passing." The locked-decisions documentation states this limitation explicitly rather than leaving a reader to discover it independently.

A second fact worth naming, evidenced directly by the model performance table: all six trained model versions — glmnet and XGBoost, across v1, v2, and v3 — are recorded as approved. The recall floor never actually rejected a version. This does not mean the floor is meaningless; it means the floor functioned as a minimum sanity check rather than a selection mechanism across this project's history. What actually drove the version-to-version iteration was not the recall gate — every version cleared it from the start — but the AUC-ROC diagnostic work documented in Section 6: the three rounds pursued genuine signal, not gate compliance, because the gate was never the binding constraint.

A related, distinct field makes the same point from a different angle: alongside the recall-based `approved` flag, the model registry separately tracks clinical signoff status, which `project_summary.md` reports as "PENDING" across all six model versions — an explicit acknowledgment that recall-floor approval and clinical readiness are tracked as two different fields precisely because they are two different things.

### Category 3 — Fairness Evaluation

| Dimension | Recall Gap | Flagged? |
|---|---|---|
| Gender (F vs. M) | 1.2 percentage points | No |
| Insurance type | 0.7 percentage points | No |
| Race | 87 percentage points | Yes — exceeds the 15-percentage-point flagging threshold |

**Stated fact:** the 87-percentage-point recall gap on race — 13.0% for the Hispanic/Latino-Salvadoran subgroup against 100% for the "unable to obtain" subgroup — is a directly measured result, logged as one of 19 rows in the `fairness_reports` governance table.

**Justified inference, not a stated fact:** the source material's own explanation is that the gap most likely reflects thin representation in the 100-patient real source producing statistical noise, rather than the model learning discriminatory patterns. The word "most likely" is the source material's own hedge, preserved here rather than upgraded to certainty. This document does not treat "noise, not learned discrimination" as established — only as the most plausible explanation currently available, distinguishable from a stated fact by the fact that no test in the source material actually rules out the alternative.

**An explicit limitation of the evaluation itself, not of the model:** subgroups with fewer than 30 members were excluded from flagging entirely, because a recall estimate from a very small subgroup is too unstable to interpret reliably. This means the fairness evaluation has a documented blind spot — very small subgroups are not evaluated for disparity at all, not because they are known to be treated fairly, but because there isn't enough data to check. That gap is disclosed in the methodology rather than silently present.

### Category 4 — Honest Disclosure, as Its Own Category

Honest disclosure is evaluated here as a fourth, independent category — not a summary of the three above, and not a synonym for any of them. It is a property of how results were reported, not a property of the results themselves; a model can be disclosed honestly and still perform modestly, just as a model could in principle perform well while being reported dishonestly. The two are orthogonal.

**What this project's disclosure certifies:** three specific choices, each documented in the source material rather than reconstructed here. First, the model's modest AUC-ROC (0.566) is reported with its root cause explained — the 100-patient signal ceiling — rather than reported as a bare number or omitted. Second, the fact that a gate checking only recall is gameable by threshold is stated in the governance documentation itself, alongside the gate. Third, the test-set-adaptive-tuning risk from the three Diagnostic Rounds (Section 6) is named explicitly, with iteration stopped and that stopping point disclosed. The source material's own framing states the underlying principle directly: a reported result of 0.566 with full explanation is treated as more credible than a reported result of 0.85 without one.

**What honest disclosure does not certify:** it does not certify that performance is good, that the approval gate is well-designed, or that fairness has been achieved — disclosure makes those results checkable, it does not improve them. Nor does it certify completeness: disclosure covers what the team identified as a limitation, which is not the same guarantee as there being no undiscovered limitations. Honest disclosure is a claim about transparency, not a claim about quality.

### Holding the Four Categories Together

The four categories, held apart rather than collapsed, describe this specific pipeline precisely: performance is modest, governance approval is true but narrowly scoped, fairness evaluation found one flagged gap with an honestly-hedged explanation, and disclosure of all three is complete as far as the source material shows. None of the four substitutes for any other — a reader relying on "approved = TRUE" alone would miss both the modest performance and the fairness flag; a reader relying on the fairness flag alone would miss that governance approval and honest disclosure both still hold. That is the case for keeping them apart, made concrete rather than asserted.

### Candidate Material for Section 11

- Treating performance, governance approval, fairness evaluation, and honest disclosure as four independent analytical axes, rather than proxies for one another — a general evaluation discipline, not specific to this model or domain.
- The observation that a governance gate checking a single metric is gameable by threshold manipulation, and that disclosing a gate's own weakness alongside the gate itself is a specific, applicable documentation habit.
- The fact/inference/limitation triage applied to the fairness finding — distinguishing a measured gap, a hedged explanation for it, and a disclosed blind spot in the evaluation method, as three separate things rather than one undifferentiated "fairness result."
- The observation that a gate every trained version happened to clear functioned as a sanity check rather than a selection mechanism — worth naming explicitly whenever a future project designs its own approval criteria, so the gate's actual role is chosen deliberately rather than assumed.

## Acceptance Criteria

1. Every reported metric — Recall, Precision, AUC-ROC, PR-AUC — is stated with both what it certifies and what it does not certify, not as a bare number.
2. Model performance, governance approval, fairness evaluation, and honest disclosure are treated as four explicitly separate categories, with a stated reason they are kept apart rather than collapsed.
3. The fairness finding is decomposed into a stated fact, a justified inference, and an explicit limitation, each labeled as such.
4. The governance approval gate's own documented weakness is stated, along with the evidenced fact that all six trained versions passed it.
5. Honest disclosure is evaluated as a property of reporting, not of results, with that distinction stated explicitly.
6. Candidate material for Section 11 is flagged without performing Section 11's formal extraction here.
7. No metric, figure, or claim appears without a citation to one of the three authoritative technical documents.

## Verification Checklist

- [x] Each of the four metrics has a stated "certifies / does not certify" pair
- [x] The four analytical categories — performance, approval, fairness, disclosure — are each given their own explicit treatment, not blended
- [x] The fairness finding is explicitly triaged into fact, inference, and limitation
- [x] The approval gate's gameable-by-threshold weakness and the six-of-six-approved fact are both stated
- [x] Honest disclosure is explicitly framed as orthogonal to result quality, not a summary of it
- [x] A candidate-material list for Section 11 is present and appropriately deferential
- [x] Confirmed by Kingsley before Section 8 begins

---

**Section 7 of 12 complete. Approved 2026-08-06.**

---

# 8. Testing & CI/CD Verification

## Purpose

Section 8 documents the pipeline's test suite — what it verifies, how it's organized, and what a passing result actually certifies. It exists as a distinct section from Section 7 for a specific reason, established there and completed here: Section 7 evaluated whether the model performs well, is approved, is fair, and was honestly reported; none of those four categories asks whether the surrounding software is correct. A model with excellent statistical properties running inside broken code would still be a broken system, and a model with modest statistical properties — as this one has, per Section 7 — running inside thoroughly-verified code is still a thoroughly-verified system. Software correctness is a fifth, independent category, not a restatement of the other four.

This section also carries a specific evidentiary caution forward from Section 3: the 71-test headline and the visible per-file test count do not reconcile, and that is not resolved here. Section 3 already applied WF-000's Drift Detection Procedure to this exact question and found it outside what the procedure can resolve, because the candidate canonical source is internally inconsistent rather than merely disagreed-with. This section reports what is and isn't established about that figure; it does not attempt a second reconciliation.

## Scope

**In scope for this section:** the test suite's composition and verification targets, the unit/integration distinction, the CI/CD mechanism that enforces it, what a passing suite certifies and does not certify, and the specific evidentiary limitations already identified around the 71-test figure.

**Out of scope for this section** (deferred to later sections):
- Deployment targets and live operational infrastructure — Section 9
- The governance tables' structure and purpose in depth — Section 10
- Model performance, governance approval, and fairness evaluation — Section 7 (already covered)

## Outputs

### The 71-Test Figure: What's Established and What Isn't

Section 3 established this precisely, and it is restated here only as a pointer, not re-derived: all three authoritative documents state "71 tests, 0 failures" identically, and `project_summary.md` further breaks this down as 55 unit and 16 integration tests. The individually-listed test files — `test_schema_validation.R` (7), `test_api_core.R` (8), `test_rag_retrieval.R` (5), `test_governance_helpers.R` (5), and `test_pipeline_e2e.R` (5) — sum to 30, not 71, and the unit-file subset (7+8+5+5=25) does not match the stated 55 either. Per Section 3, this is not classifiable as Drift or Gap under WF-000 §4's procedure, because there is no dependent-versus-canonical disagreement to classify — the candidate canonical source states both numbers itself, consistently, without reconciling them. No new evidence has been introduced since Section 3 that would change this. What follows treats "71 tests, 0 failures" as the reported headline — repeated identically across all three documents, and therefore the higher-confidence figure — while treating the specific per-category breakdown as unresolved, not fabricated to fill the gap.

### What the Test Suite Verifies, By Category

| Test File | Tests | What It Verifies |
|---|---|---|
| `test_schema_validation.R` | 7 | That the canonical schemas' type-casting, null checks, and referential integrity rules — Stage 2's ingest gate — behave as specified |
| `test_api_core.R` | 8 | That every API route and response contract behaves as specified |
| `test_rag_retrieval.R` | 5 | That guideline retrieval returns clinically relevant results for given inputs |
| `test_governance_helpers.R` | 5 | That the seven governance write functions populate the audit tables correctly |
| `test_pipeline_e2e.R` (integration) | 5 | Deterministic predictions; predict-versus-explain consistency; clinically relevant RAG retrieval at the system level; audit-row accumulation; all eight governance tables intact |

### Unit Tests versus Integration Tests

Unit tests exercise individual components in isolation — the schema validators, the API's core business-logic functions, the retrieval logic, the governance write functions — each checked independently of the others. This is possible specifically because of a design decision already documented in Section 5: the `*_core()` pattern separates business logic from HTTP routing precisely so that logic can be called and tested directly, without running a server. The unit tests are the direct beneficiary of that architectural choice, not an independent capability.

Integration tests exercise the assembled system as a whole — a request entering the API, a prediction being generated, an explanation being generated consistently with that prediction, a RAG retrieval happening for the same patient, and an audit row being written, all in the same test. `test_pipeline_e2e.R`'s five tests check exactly this: deterministic predictions, predict-versus-explain consistency, clinically relevant RAG retrieval, audit-row accumulation, and that all eight governance tables remain intact through the process. This is why, as Section 6 already established, the integration tests specifically could not have been finalized before the components they integrate — the model, the API, the governance tables — all existed; that dependency is real and stage-specific, distinct from the looser "testing came last" reading Section 6 explicitly declined to make. Nothing in the source material states when unit tests for earlier components were first written — only when the complete, 71-test suite was finalized as a whole. This section preserves that same distinction rather than collapsing it.

### The Windows DuckDB Connection-Locking Issue, From the Testing Angle

Section 4 documents this as a constraint on the build; here it is treated specifically as a property of the testing process itself. The failure — DuckDB file-locking conflicts — is described in the source material as appearing only during rapid sequential test execution, not during normal, slower, single-session use. This means the problem was not latent in the pipeline generally; it was surfaced specifically by the act of running many tests quickly in succession, which is itself informative: a comprehensive automated test suite can expose infrastructure problems that occasional manual use would not.

The fix — a connection-singleton pattern in `tests/unit/setup.R` and `tests/integration/setup.R`, with `withr::defer()` teardown and `.restore_test_singleton()` called after each `source()` call that resets overrides — is test infrastructure, not a change to the pipeline's production code path. It solves a problem that exists because of how the tests are run, not because of how the pipeline itself operates in production.

### CI/CD as the Automated Gate

The nine-step GitHub Actions pipeline — checkout, R setup, `renv` restore, lint, script-existence check, metadata validation, the recall gate, the locked-decisions policy check, and unit tests — runs on every push to `main` and on every pull request targeting `main`. Any step failing blocks the deployment that would otherwise follow.

One precise point is worth making rather than glossing over: the pipeline's own nine-step enumeration names its final step as "unit tests (testthat, Stage 9)" specifically, while a separate statement elsewhere in the same source material says all 71 tests run in CI on every push. These are not necessarily in conflict — a single `testthat` invocation can run both unit and integration suites under one label — but the two mentions use different levels of precision, and this document does not resolve that gap by assuming the more expansive reading is correct. What is stated consistently, in both versions, is that test execution is part of the CI gate and that a failure blocks deployment; the exact scope of what runs in that specific step is not stated with full consistency.

### What a Passing Test Suite Certifies — and Does Not

| A Passing Test Suite Certifies | It Does Not Certify |
|---|---|
| That the specific behaviors the 71 tests check — schema rules, API contracts, governance writes, retrieval relevance, end-to-end consistency — behave as specified, under the conditions tested | Model quality: AUC-ROC, precision, recall, and PR-AUC are Section 7's territory entirely — a test suite verifies that code runs correctly, not that a model's predictions are statistically strong |
| That a specific, previously-encountered defect (the Windows DuckDB locking issue) does not recur under the conditions the tests exercise | Clinical readiness: explicitly disclaimed throughout the source material as "FOR PORTFOLIO DEMONSTRATION ONLY — NOT FOR CLINICAL USE," a status no test result changes |
| | Fairness: `test_governance_helpers.R` verifies that fairness reports are *written* to the governance table correctly — it does not evaluate whether the underlying fairness finding (Section 7's 87-percentage-point race gap) is itself acceptable |
| | Completeness of coverage: 71 passing tests means 71 specific checks passed, not that every possible failure mode has been exercised — code paths the suite does not touch are simply untested, not verified-safe |

### Local Verification versus CI Verification

`README_portfolio.md` documents both `testthat::test_dir("tests/unit")` and `testthat::test_dir("tests/integration")` as commands a developer can run directly, under a Testing section distinct from the Run Locally instructions. The CI pipeline runs test execution automatically, as one of its nine steps, specifically on push and pull-request events targeting `main` — not on demand, the way the local commands are.

The relationship the source material directly supports is this: local test runs are developer-initiated and can happen at any point during development; CI test runs are event-triggered and function as a gate — per the CI/CD documentation, "any gate fails = deployment blocked." What the source material does not describe is whether the CI environment differs from local development environments in ways relevant to the platform-specific issues documented elsewhere in this pipeline — the Windows DuckDB and Windows XGBoost issues, both traced in Section 4 to the local development environment. That is an unaddressed detail, not a stated fact in either direction, and it is noted here as a gap rather than filled with an assumption about what CI infrastructure typically looks like.

### Candidate Material for Section 11

- The observation that a comprehensive automated test suite can surface infrastructure problems — the Windows DuckDB locking issue — that occasional manual use would not: a reason to build and run a test suite early and often, not only as a final gate.
- The `*_core()` separation-of-concerns pattern, already flagged as candidate material in Section 5's design-decision discussion, reinforced here from the testing side: an architectural choice made for testability, and the unit tests that specifically depend on it.
- The certifies/does-not-certify treatment of test results, applied here the same way Section 7 applied it to model metrics — a general evaluation discipline for verification of any kind, not specific to software or to this project.
- The distinction between a gate that blocks deployment on failure (CI) and a check a developer runs at will (local) — worth naming explicitly whenever a future project designs its own verification workflow, so the two roles are chosen deliberately rather than conflated.

## Acceptance Criteria

1. The 71-test headline and the per-file discrepancy are reported exactly as Section 3 established them, without a new reconciliation attempted here.
2. Every test file's verification target is stated from the source material, not inferred beyond what's documented.
3. The unit/integration distinction is explained with reference to the architectural decision (Section 5) that makes unit testing possible, and the Section 6 timing distinction is preserved rather than collapsed into "testing happened last."
4. The Windows DuckDB issue is treated specifically as a testing-infrastructure matter here, distinct from its treatment as a build constraint in Section 4.
5. What a passing test suite certifies and does not certify is stated explicitly, with model quality, clinical readiness, fairness, and coverage completeness each named as things it does not certify.
6. The CI step's own "unit tests" label versus the "71 tests run in CI" statement elsewhere is flagged as an unresolved precision gap, not silently harmonized.
7. The local-versus-CI environment question is stated as an unaddressed gap rather than filled with outside knowledge about typical CI infrastructure.
8. Candidate material for Section 11 is flagged without performing Section 11's formal extraction here.

## Verification Checklist

- [x] The 71-test figure is reported per Section 3's already-established finding, with no new reconciliation attempted
- [x] Each test file's verification target is stated from source material
- [x] Unit/integration distinction references Section 5's `*_core()` pattern and preserves Section 6's timing distinction
- [x] The Windows DuckDB issue is treated as a testing-infrastructure matter, cross-referenced to, not duplicated from, Section 4
- [x] A certifies/does-not-certify table states what a passing suite does and does not certify, covering model quality, clinical readiness, fairness, and coverage completeness
- [x] The "unit tests" CI-step label versus "71 tests run in CI" statement is flagged as an unresolved precision gap
- [x] The local-vs-CI environment question is stated as an unaddressed gap, not filled with outside knowledge
- [x] Candidate material for Section 11 is present and appropriately deferential
- [x] Confirmed by Kingsley before Section 9 begins

---

**Section 8 of 12 complete. Approved 2026-08-06.**

---

# 9. Deployment & Live Operations

## Purpose

Section 9 documents where the pipeline actually runs, how it got there, and what that establishes — distinct from Section 5's account of why the architecture was designed the way it is, and distinct from Section 8's account of what the automated test suite verifies before code is considered mergeable. Deployment is a third thing: the fact of a system being live and reachable, treated here as its own category with its own evidentiary boundaries, rather than as a natural consequence of good architecture or passing tests.

The central discipline of this section is a boundary already established in Sections 4 and 7 and reaffirmed here rather than re-derived: this pipeline being deployed and reachable does not make it production-ready or clinically ready. Those are different claims, and the source material itself keeps them separate — the disclaimer appears not only in documentation but in the live API's own response payload. This section preserves that separation rather than letting "it's live" quietly imply "it's ready."

## Scope

**In scope for this section:** deployment targets and infrastructure, API and dashboard deployment specifically, the deployment workflow and its relationship to the CI/CD gate documented in Section 8, live operational boundaries, and what deployment does and does not establish about readiness.

**Out of scope for this section** (deferred to other sections):
- Why the architecture was designed as it was — Section 5
- What the automated test suite verifies — Section 8
- The governance tables' structure and the monitoring script's mechanics — Section 10
- Model performance, approval, and fairness — Section 7

## Outputs

### Deployment Targets and Infrastructure

Three live targets are documented, each already registered in WF-000 §3 (Artifacts 5–7) with its own canonical source and sync trigger: the GitHub repository, the live dashboard, and the live API. This section adds the rationale and operational narrative those registry entries deliberately exclude, per WF-000 §3's own acceptance criteria that a registry row describes a triggering condition, not synchronization mechanics.

**The three registered targets:**

| Target | Platform | What Runs There | WF-000 Registration |
|---|---|---|---|
| GitHub Repository | `github.com/Kayterthesly/r-healthcare-readmission` | Source code and version history | Artifact 5 |
| Dashboard | shinyapps.io | A five-tab Shiny application — Pipeline Overview, Patient Risk, Model Performance, Fairness Analysis, Governance Monitor | Artifact 6 |
| API | Railway (Docker container) | Four Plumber endpoints — `/health`, `/predict`, `/explain`, `/rag/summary` | Artifact 7 |

**Storage infrastructure supporting them** — not separately registered in WF-000 §3; documented here because it supports the API and dashboard above, not as a fourth registered target:

| Storage | Platform | Role |
|---|---|---|
| Production | Backblaze B2 | Nine Parquet files, 82 MB, S3-compatible |
| Local development | MinIO (Docker) | The same schema, used during development per Section 5's Design Decision 1 |

`Dockerfile` and `railway.toml` are named in the repository structure as the Railway-specific deployment configuration; their contents are not described further in the source material beyond their stated purpose.

### API Deployment

The API is deployed to Railway as a Docker container, reachable at `r-healthcare-readmission-production.up.railway.app`. Its `/health` endpoint reports status, model version, index version, a timestamp, and the list of available endpoints — a directly-callable way to verify the deployment is live, demonstrated in `README_portfolio.md`'s own quick-test example (`curl .../health`).

Every `/predict` response — including the example shown in the API reference — carries a `disclaimer` field reading "FOR PORTFOLIO DEMONSTRATION ONLY — NOT FOR CLINICAL USE." This is worth naming precisely because of where it appears: not only in README prose, which a user could reasonably skip, but in the payload of the live system's own response, which any caller — human or automated — receives on every call. That is a stronger form of disclosure than documentation alone, and it is treated as a stated fact here because it appears in the API reference's own example output, not because this document infers it should be there.

One implementation detail, not itself a deployment decision: the model is loaded once at process startup rather than per-request, a design choice already documented in Section 5.

### Dashboard Deployment

The dashboard is deployed to shinyapps.io using what the source material describes as a pre-computed static bundle — a governance snapshot — for its non-patient-specific content, combined with live `httr2` POST calls to the Railway API for patient-specific predictions. It does not connect to DuckDB or read local files at runtime; per the source material, this was a deliberate choice specifically so the deployed dashboard has no dependency on the pipeline's local data infrastructure.

One boundary of the provider-agnostic design principle established in Section 5 is worth stating precisely rather than assuming it holds everywhere: the dashboard's connection to the Railway API is described as a hardcoded URL, not a configuration value. Section 5's swappable-storage design applies to the data layer specifically; nothing in the source material indicates the same swappability was extended to the dashboard's connection to the API. This is not a criticism — the source material gives no indication the URL needed to change — but it is a distinct fact from the data layer's portability, and this document does not blur the two.

### Deployment Workflow: Automated versus Manual

The API and the dashboard are deployed through genuinely different workflows, and this section keeps that distinction explicit rather than describing "deployment" as one uniform process.

**The API:** `README_portfolio.md` states that Railway auto-deploys on push to `main`, triggered by `git push origin main` — an automated, push-triggered mechanism.

**The dashboard:** deployment is an explicit, manually-run command — `rsconnect::deployApp(appDir = "dashboard", appName = "healthcare-readmission-pipeline", account = "your-shinyapps-account")` — described under its own "Deploy Dashboard to shinyapps.io" heading, distinct from the API's automated path. Nothing in the source material describes the dashboard's deployment as triggered by a push event.

### The Relationship Between Deployment and the CI/CD Gate — An Open Evidence Gap

Section 8 documents a nine-step GitHub Actions pipeline that runs on push to `main` and blocks on failure. This section's API deployment is also described as triggered by push to `main`, via Railway's own auto-deploy. Both are real, stated facts. What is not established by the source material is how the two relate to each other.

The CI/CD slide's own diagram ends in a checkmark labeled "Deploy," which could be read as the CI workflow's actual final action — meaning Railway's deployment is gated by the nine checks completing successfully. But the technical enumeration of the nine steps, given separately in the same source material, stops at "unit tests" and does not name a deploy step explicitly; and the API's own deployment instructions describe Railway's auto-deploy as its own mechanism, triggered by the push event itself, without stating that it waits on the GitHub Actions workflow's result. Both readings are consistent with what's written; neither is confirmed by it. This document does not resolve the ambiguity by choosing the more reassuring interpretation — that a failing test run always prevents a live deployment — because the source material does not state that connection explicitly. It is recorded here as an open evidence gap, not as an established safeguard.

### Live Operational Boundaries

The source material states a sub-two-second response target for a live prediction and discharge recommendation together, presented as a headline characteristic of the deployed system rather than as a benchmark with a described testing methodology attached — this document reports it as a stated claim in the source material, not as independently verified here.

Beyond reachability and that stated latency figure, the source material does not describe uptime guarantees, a service-level agreement, load-testing results, or rate-limiting behavior for either the API or the dashboard. These are not claimed as failures of the deployment; they are simply not addressed in the three authoritative documents, and this document notes their absence rather than assuming a typical production posture on the deployment's behalf.

### What Deployment Establishes — and Does Not

| Deployment Establishes | Deployment Does Not Establish |
|---|---|
| The API and dashboard are reachable over the internet at stated URLs, and the API's `/health` endpoint reports itself as operational | Clinical readiness — explicitly and repeatedly disclaimed, including in the API's own response payload |
| A sub-two-second response time is claimed for the live prediction-and-recommendation flow | That this figure has been independently verified, load-tested, or measured under production-scale traffic — the source material states it without a described methodology |
| The API has an automated, push-triggered deployment path (Railway); the dashboard has an explicit manual one (`rsconnect::deployApp`) | That the API's automated deployment is gated by Section 8's CI checks passing — see the open evidence gap above |
| The dashboard operates independently of the pipeline's local data infrastructure at runtime | Uptime, an SLA, load-testing results, or rate-limiting behavior — none of these are addressed in the source material for either target |

### Relationship to WF-000's Registry

WF-000 §3 already registers the GitHub repository, the live dashboard, and the live API as Artifacts 5, 6, and 7 respectively, each with its own canonical source (the live deployment itself) and sync trigger ("redeploy, or the service is moved or retired"). This section does not re-register these artifacts or restate their sync mechanics — per WF-000 §6.4, WF-001 cannot override that machinery, and per Section 2's inheritance statement, it doesn't need to. What this section adds is the narrative context WF-000's own acceptance criteria deliberately excluded from the registry rows themselves: why these targets, why this workflow, and what the deployment facts do and do not establish. If any of these three artifacts is ever moved or retired, that event is a Registry Change under WF-000 §4's trigger taxonomy, handled through WF-000's own Verification Workflow — not through an amendment to this section.

### Candidate Material for Section 11

- The observation that different components of one system can warrant genuinely different deployment automation — the API's push-triggered path and the dashboard's manual one are both reasonable, but they are different, and a future project should choose deliberately which components need which workflow rather than assume uniformity.
- The finding that a portability principle established for one layer (Section 5's swappable storage) does not automatically extend to every other connection in the system (the dashboard's hardcoded API URL) — a caution against assuming a design property generalizes further than it was actually built to.
- Embedding a use-boundary disclaimer directly in a live system's own response payload, not only in its documentation, as a stronger and harder-to-miss form of disclosure — directly extending the honest-disclosure discipline from Sections 4, 6, and 7 into operational, not just documentary, practice.
- The general lesson from the CI/deploy relationship gap: when two automated systems trigger on the same event, their relationship — sequenced, gated, or independent — needs to be explicitly designed and documented, not left to be inferred from the fact that they happen to share a trigger.

## Acceptance Criteria

1. Deployment targets, API deployment, and dashboard deployment are each documented with citations to the source material, with no claim about their contents beyond what's stated.
2. The API's automated deployment workflow and the dashboard's manual one are kept explicitly distinct, not described as one uniform "deployment process."
3. The relationship between Railway's auto-deploy and Section 8's CI/CD gate is stated as an open evidence gap, not resolved by assuming the more reassuring reading.
4. Live operational claims (the sub-two-second response figure) are reported as stated claims from the source material, not independently verified or treated as confirmed benchmarks.
5. Deployment is explicitly distinguished from production-readiness and clinical-readiness throughout, reinforcing the boundary established in Sections 4 and 7 rather than letting "live" imply "ready."
6. WF-000 §3's existing registration of these three artifacts is referenced, not re-derived or duplicated.
7. Architectural rationale (Section 5), testing verification (Section 8), and governance-table depth (Section 10) are not duplicated here.
8. Candidate material for Section 11 is flagged without performing Section 11's formal extraction here.

## Verification Checklist

- [x] Deployment targets and infrastructure are documented with source citations, with unknown details (Dockerfile/railway.toml contents) noted as unknown rather than assumed
- [x] API and dashboard deployment workflows are kept explicitly distinct — automated versus manual
- [x] The Railway-auto-deploy-versus-CI-gate relationship is stated as an open evidence gap, not silently resolved
- [x] The sub-two-second response claim is attributed to the source material as a stated claim, not treated as independently verified
- [x] "Deployed" is never equated with "production-ready" or "clinically ready" anywhere in this section
- [x] WF-000 §3's registration of Artifacts 5–7 is referenced by citation, not restated or duplicated
- [x] No content duplicates Section 5's design rationale, Section 8's testing verification, or Section 10's governance-table depth
- [x] Candidate material for Section 11 is present and appropriately deferential
- [x] Confirmed by Kingsley before Section 10 begins

---

**Section 9 of 12 complete. Approved 2026-08-06.**

---

# 10. Governance & Monitoring Integration

## Purpose

Section 10 documents the pipeline's own internal governance and monitoring system — the eight append-only DuckDB tables that record its operational history, the locked-decisions document that records its governance rules, the monitoring script that reads both, and the policy checks that enforce them. This is distinct from every prior section that touched governance in passing: Section 5 established governance as an architecturally first-class layer without detailing its contents; Section 7 evaluated the fairness *finding* the governance layer recorded, not the recording mechanism itself; Section 8 verified that the governance write functions behave correctly in tests, not what they write or why. This section is where the layer itself — its tables, their structure, and what they can and cannot be trusted to show — gets documented in full for the first time.

The central discipline carried forward from Section 2 is a terminology boundary this section depends on holding: "governance," in this document, means two different things depending on which system is meant. WF-000's governance concerns identity-artifact consistency — drift, canonical sources, approval authority. This section's governance concerns the pipeline's own audit trail, policy enforcement, and monitoring — a property of the system being documented, not of this document's own oversight. The two do not merge here.

## Scope

**In scope for this section:** the eight governance tables — what each records and why — the locked-decisions document as distinct from the tables, the monitoring layer and what it can and cannot currently demonstrate, the policy-check mechanism at the level needed to complete this picture, and what the governance/monitoring layer establishes versus does not establish.

**Out of scope for this section** (covered elsewhere, not duplicated here):
- System architecture and why governance sits where it does structurally — Section 5
- The fairness finding itself and what it means — Section 7
- Test verification of the governance write functions — Section 8
- Deployment infrastructure — Section 9

## Outputs

### The Eight Governance Tables

Per Section 3's established finding, the pipeline maintains eight governance tables — not seven, the figure `presentation_nontechnical.md` states once and Section 3 classified as Drifted against the other two authoritative documents, which name all eight directly and consistently.

| Table | Records | Write Pattern | Populated From |
|---|---|---|---|
| `ingest_metadata` | Every canonical table upload — job ID, data hash, sensitivity label, operator, timestamps | Append-only, with a PHI gate | Stage 2 |
| `feature_registry` | Every computed feature, with a leakage note per entry | Idempotent by feature name and version | Stage 3 |
| `model_registry` | Every training run, across all versions and metrics | Append-only — no version is overwritten | Stage 4 |
| `fairness_reports` | Recall and precision by subgroup, one row per subgroup | Append-only | Stage 5 |
| `rag_chunks` | The indexed guideline chunks | Overwritten on index rebuild | Stage 6 |
| `rag_index_metadata` | Index build provenance — when and how the index was built | Append-only, each rebuild traceable | Stage 6 |
| `llm_call_log` | Every LLM call, request and response stored as hashes | Append-only | Stages 6–7 |
| `predictions_audit` | Every `/predict` call — patient ID hash, input hash, model version, risk score, risk tier, trace ID, environment | Append-only | Stage 7 |

Two of the tables — `llm_call_log` and `predictions_audit` — are reported with different row counts across `project_summary.md` (3+ and 1+, respectively) and `README_portfolio.md` (8+ and 15+, respectively). Both figures are qualified with "+" in both documents. Given both tables are append-only and accumulate rows through ongoing use, this reads as two snapshots taken at different points in the pipeline's operation, not a contradiction — the same reasoning already applied to similarly qualified figures during the original evidence review. This document does not treat it as a new finding requiring Section 3-style classification.

`fairness_reports`, specifically, has a human-readable counterpart distinct from the table itself: a markdown report (`models/artifacts/fairness_report_xgboost_v3.md`), described in the source material as containing the fairness stratification detail Section 7 evaluated. The table and the report are not the same artifact — the table is the structured, queryable log; the report is the written summary derived from it.

Seven write functions, documented in `r_scripts/governance_helpers.R`, populate these eight tables. The source material does not specify which function writes to which table, or whether one function writes to more than one table — this document notes that as an unaddressed implementation detail rather than assuming a specific one-to-one mapping that isn't stated.

### Locked Decisions, Distinguished from Governance Tables

The pipeline's `docs/00_locked_decisions.md` is a separate artifact from the eight governance tables, and Section 2's glossary already draws this distinction formally: a Locked Decision is a governance *determination* — a rule about what a flag means, what threshold applies, what counts as acceptable — while a Governance Table is an operational *record* of what actually happened. The recall-floor certification boundary discussed at length in Section 7 (`approved = TRUE` certifies Recall ≥ 0.85 only) is a Locked Decision. The `model_registry` row logging a specific training run's actual recall value is a Governance Table entry. One states the rule; the other records an instance.

The source material describes the locked-decisions document as containing 13 sections, enforced in part by the policy-check mechanism described below. Its specific contents beyond what Sections 4 and 7 have already cited — the recall-floor scope, the gameable-gate disclosure — are not elaborated further in the three authoritative documents, and this document does not reconstruct the remaining sections from inference.

### The Monitoring Layer

`r_scripts/08_monitoring.R` reads all eight governance tables and produces a timestamped health report, written to `logs/`. Per the source material, it computes four things: model health (the count of approved models and each one's recall-gate status), prediction volume together with Population Stability Index drift, a fairness summary, and LLM call statistics — alongside a governance-completeness check across the eight tables.

The PSI drift mechanism is worth treating precisely rather than as a single fact, because the source material itself reports two different things about it that should not be collapsed into one. First, the *mechanism* is built and documented: PSI thresholds are defined (below 0.10 is stable, 0.10 to 0.25 warrants investigation, above 0.25 warrants retraining), and the monitoring script is described as computing it. Second, and separately, an actual monitoring report excerpt in the source material shows the drift result as "insufficient data," with a stated minimum of 30 predictions needed for a reliable PSI estimate, and the framework described as "ready" rather than as having produced a validated reading. The mechanism existing and the mechanism having been exercised against enough real data to say anything are two different claims. This document states only the first as established; the second — that PSI drift detection actually works as intended on live traffic — is not something the source material demonstrates, because the volume threshold for a reliable estimate had not yet been reached at the time of the reported snapshot.

Beyond this, the source material does not describe uptime alerting, automated anomaly notification, alert routing to a person or system, an incident-response process, or a data-retention policy for the governance tables. These are not claimed as absent from the actual system — only as unaddressed in the three authoritative documents, which is the boundary this document is limited to.

### Policy Checks

The six-policy check (`infra/policies/model_policy_check.R`) — approved model, recall gate, leakage notes, decisions-document sections, metadata JSON schema, and required scripts — is already named in Sections 6 and 8 as part of the CI/CD gate's enforcement mechanism. It is not re-derived here; its role in this section is only to complete the picture of how the governance layer connects to enforcement: the policy check reads the same locked-decisions document and touches several of the same governance tables documented above — the recall gate reads `model_registry`; the leakage-notes check reads `feature_registry`.

### What the Governance/Monitoring Layer Establishes — and Does Not

| The Governance/Monitoring Layer Establishes | It Does Not Establish |
|---|---|
| Every ingestion, feature, training run, fairness stratification, RAG index build, LLM call, and prediction is logged, with patient data and raw inputs stored only as hashes | Clinical governance or regulatory approval — this remains a portfolio demonstration, and no volume of internal audit logging changes that status |
| A drift-monitoring mechanism (PSI) is built, with defined thresholds | That drift monitoring has been validated against enough live traffic to say it works as intended — the source material reports insufficient data for a reliable estimate as of the reported snapshot |
| The `fairness_reports` table records the 87-percentage-point race disparity Section 7 evaluated, as one of 19 logged subgroup rows | That the disparity is acceptable, resolved, or explained with certainty — a table recording a finding is not the finding being acceptable; Section 7's fact/inference/limitation triage on that finding still holds |
| A six-policy check enforces specific, named invariants against the locked-decisions document and specific governance tables | Uptime alerting, anomaly notification routed to a person, incident response, or a stated data-retention policy — none of these are addressed in the source material |

### Pipeline Governance versus WF-000 Governance

Section 2 already drew this boundary and it is reaffirmed rather than re-derived here: everything documented above — the eight tables, the locked-decisions document, the monitoring script, the policy checks — is Process Artifact-level content in WF-000's sense, internal to the pipeline's own operation, not itself an identity-stack claim WF-000 governs. It becomes subject to WF-000 §4's Drift Detection only at the point a specific figure from it is restated externally as a public claim — the AUC-ROC 0.566 quoted on a resume, for instance, not the `model_registry` row it came from. This section's own content does not require WF-000 registration; it documents a system, not a claim WF-000's registry tracks.

### Candidate Material for Section 11

- The distinction between a governance mechanism *existing* (PSI drift detection, built and threshold-defined) and a governance mechanism *proven in operation* (requiring sufficient real data, not yet reached) — a general caution against treating "the monitoring is built" as equivalent to "the monitoring works," applicable to any system with a similar cold-start data requirement.
- The Locked Decision / Governance Table distinction itself — separating a rule about what a result means from the record of the result — as a reusable documentation discipline for any project with both a governance policy and an operational audit trail.
- The observation that a governance table recording a finding is not the same as the finding being acceptable, applied here to fairness specifically but stated as a general principle: logging is not evaluation, and the two should not be allowed to imply each other.
- The append-only-versus-overwrite design choice varying by table (`rag_chunks` overwrites on rebuild; every other table is append-only) — a reminder that a uniform "log everything, always" policy isn't always the right choice; the write pattern should match what the specific data actually needs to preserve.

## Acceptance Criteria

1. All eight governance tables are documented individually — what each records, its write pattern, and which stage populates it — with the seven-versus-eight table count treated per Section 3's established finding, not revisited.
2. Locked Decisions and Governance Tables are kept explicitly distinct throughout, per Section 2's definitions, with a concrete example of each.
3. The PSI drift-monitoring mechanism's existence is distinguished explicitly from its having been validated against sufficient live data — the two are not collapsed into one claim.
4. No claim states or implies that the governance layer constitutes clinical governance, regulatory approval, or evidence that the recorded fairness disparity is acceptable.
5. Monitoring capabilities not addressed in the source material — alerting, anomaly routing, incident response, retention policy — are identified as gaps, not filled from general MLOps practice.
6. Content already covered in Sections 5, 7, 8, and 9 is cross-referenced, not duplicated.
7. The boundary between pipeline governance and WF-000 governance, established in Section 2, is reaffirmed rather than re-argued.
8. Candidate material for Section 11 is flagged without performing Section 11's formal extraction here.

## Verification Checklist

- [x] All eight governance tables are individually documented with write pattern and populating stage
- [x] The seven-versus-eight count is handled per Section 3's finding, with no new investigation opened
- [x] Locked Decision and Governance Table are kept explicitly distinct, each with a concrete example
- [x] The PSI mechanism's existence and its unvalidated-against-sufficient-data status are both stated, not merged into one claim
- [x] No sentence states or implies clinical governance, regulatory approval, or that the fairness disparity is resolved
- [x] Unaddressed monitoring capabilities (alerting, incident response, retention) are named as gaps
- [x] Sections 5, 7, 8, and 9 content is cross-referenced, not restated
- [x] The pipeline-governance/WF-000-governance boundary from Section 2 is reaffirmed, not re-derived
- [x] Candidate material for Section 11 is present and appropriately deferential
- [ ] Confirmed by Kingsley before Section 11 begins

---

**Section 10 of 12 drafted. Pending confirmation before Section 11 begins.**

---

# 11. Reusable Engineering Patterns — The Operating Model for WF-002–WF-006

*Pending. Will formally define the Engineering Pattern Extraction process — transforming the project-specific decisions documented in Sections 4–10 into named, reusable Engineering Patterns, per the Section 2 definitions, explicitly designed for reuse in WF-002 and future workflows. These patterns, together with the objectives, architecture, and governance recorded elsewhere in this document, constitute the Operating Model that future WF-00X workflows inherit as part of the Kingsley Operating System.*

---

# 12. Relationship to WF-000 & Compliance Audit

*Pending. Will confirm no duplication of WF-000's governance machinery, confirm registry status, and log the Master-CV.md §TP-001 correction as an action item.*