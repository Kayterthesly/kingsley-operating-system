# WF-001 — Healthcare Readmission Pipeline: Operating Model & Reusable Engineering Workflow

Version: Not yet assigned — versioning begins at 0.1 upon first complete pass through every planned section (WF-000 §5.1)
Status: In Progress — Sections 1–4 approved; Section 5 drafted, pending confirmation
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
5. System Architecture & Design Rationale — **drafted, pending confirmation**
6. Engineering Process — Build Sequence & Diagnostic Discipline — pending
7. Success Metrics, Fairness & Honest Evaluation — pending
8. Testing & CI/CD Verification — pending
9. Deployment & Live Operations — pending
10. Governance & Monitoring Integration — pending
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
- [ ] Confirmed by Kingsley before Section 6 begins

---

**Section 5 of 12 drafted. Pending confirmation before Section 6 begins.**

---

# 6. Engineering Process — Build Sequence & Diagnostic Discipline

*Pending. Will document the staged build sequence and the root-cause diagnostic method demonstrated across the project — the engineering reasoning Section 11 later extracts into Engineering Patterns.*

---

# 7. Success Metrics, Fairness & Honest Evaluation

*Pending. Will document the reported metrics, what each does and does not certify, and the fairness evaluation.*

---

# 8. Testing & CI/CD Verification

*Pending. Will document the test suite and CI gate, including the unreconciled per-category test count.*

---

# 9. Deployment & Live Operations

*Pending. Will document where the system runs and the provider-agnostic storage design, cross-referenced to WF-000 §3 Artifacts 5–7.*

---

# 10. Governance & Monitoring Integration

*Pending. Will document the pipeline's internal governance layer and disambiguate it from WF-000's identity-stack governance.*

---

# 11. Reusable Engineering Patterns — The Operating Model for WF-002–WF-006

*Pending. Will formally define the Engineering Pattern Extraction process — transforming the project-specific decisions documented in Sections 4–10 into named, reusable Engineering Patterns, per the Section 2 definitions, explicitly designed for reuse in WF-002 and future workflows. These patterns, together with the objectives, architecture, and governance recorded elsewhere in this document, constitute the Operating Model that future WF-00X workflows inherit as part of the Kingsley Operating System.*

---

# 12. Relationship to WF-000 & Compliance Audit

*Pending. Will confirm no duplication of WF-000's governance machinery, confirm registry status, and log the Master-CV.md §TP-001 correction as an action item.*