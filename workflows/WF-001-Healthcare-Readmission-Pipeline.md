WF-001 — Healthcare Readmission Pipeline: Operating Model & Reusable Engineering Workflow

Version: Not yet assigned — versioning begins at 0.1 upon first complete pass through every planned section (WF-000 §5.1) Status: In Progress — Section 1 approved; Section 2 drafted, pending confirmation Author: Kingsley Akenu Architect: Claude Last Updated: 2026-08-06 Governed By: WF-000 — Academic & Professional Identity Stack (Version 1.0) Evidentiary Sources: project_summary.md · presentation_nontechnical.md · README_portfolio.md (authoritative); Master-CV.md §TP-001 (cited as drift evidence only) Registry Cross-Reference: WF-000 §3, Artifacts 5–7 (Healthcare Pipeline — GitHub Repo, Live Dashboard, Live API) — already registered

Document Map
Purpose & Problem Statement — approved
Scope, Definitions & Governance Inheritance — drafted, pending confirmation
Evidence Base & Source Reconciliation — pending
Objectives & Constraints — pending
System Architecture & Design Rationale — pending
Engineering Process — Build Sequence & Diagnostic Discipline — pending
Success Metrics, Fairness & Honest Evaluation — pending
Testing & CI/CD Verification — pending
Deployment & Live Operations — pending
Governance & Monitoring Integration — pending
Reusable Engineering Patterns — The Operating Model for WF-002–WF-006 — pending
Relationship to WF-000 & Compliance Audit — pending
1. Purpose & Problem Statement
Purpose

WF-001 establishes the structured record of how the healthcare readmission pipeline was actually built: the objectives it pursued, the constraints that bounded its design, the architecture it settled on and why, the process by which its failures were diagnosed and resolved, the metrics it achieved and precisely what those metrics do and do not certify, and the governance layer that keeps its claims honest. Per WF-000 §6.1, it does this as the first of six planned WF-00X documents, so that the documents which follow it — WF-002 through WF-006 — inherit a working pattern rather than starting from nothing.

Its function is preventive and generative, not archival. A retrospective that only describes what happened would be archival — a historical record with no forward use, no different in kind from the README it's built from. Absent WF-001, two things are true right now, both demonstrated below rather than assumed: the reasoning behind the pipeline's engineering decisions exists nowhere in a form designed to be extracted and reapplied to a different project, and the plain factual record of what was built is already drifting — in a document explicitly designated as this project's canonical source — only weeks after the project's own completion date.

Evidence 1 — the factual record has already drifted. Master-CV.md describes itself as the canonical source from which "all resumes, scholarship CVs, portfolio content, LinkedIn updates, and academic applications derive." Its entry for TP-001 (the Healthcare Readmission Prediction Pipeline) lists the technology stack as Python, FastAPI, Docker, GitHub Actions, Azure, MLflow, and DVC. The pipeline that was actually built and deployed is written in R, queries DuckDB with the httpfs extension, trains with tidymodels (XGBoost and glmnet), retrieves clinical guidelines via TF-IDF and generates discharge summaries with ellmer, serves predictions through plumber, and is deployed on Railway and shinyapps.io with object storage on Backblaze B2 — a description independently and consistently corroborated by project_summary.md, presentation_nontechnical.md, and README_portfolio.md, down to matching AUC-ROC and recall figures, matching live URLs, and matching file inventories. Master-CV.md's own "Evidence" list for TP-001 doesn't mention a live dashboard at all, despite the dashboard being a fully deployed, independently documented component of the system. Whichever account was written first, the document positioned as authoritative is the one that no longer describes the system that exists — and this holds regardless of which account eventually proves more accurate in some deeper sense; a mismatch between a designated-canonical source and the system it describes is itself the evidence.

Evidence 2 — the reasoning was never captured in reusable form to begin with. The most distinctive piece of engineering judgment in the entire project is the Stage 4 diagnostic sequence: a near-random model (AUC 0.55–0.57) was traced not to a bug in the modeling code but to a data-generation flaw several stages upstream, in Stage 1, and a subsequent suspicious spike in one feature's importance (30.6% of XGBoost's gain, concentrated in a single lab value) was proven to be a coincidental artifact of random-number consumption — not a real signal — by checking whether a simpler model agreed, and finding that it did not. That sequence of reasoning exists today only as prose describing this one incident, on this one project. Nowhere in the current record does "when a feature's importance jumps and a simpler, differently-randomized model doesn't corroborate it, suspect a coincidental artifact of the data pipeline before trusting the signal" exist as a named, portable technique — available on purpose to WF-003's agent-evaluation work, or to any future project, independent of healthcare, independent of XGBoost. The knowledge survived the project. It did not survive extraction.

These are different problems, not the same problem under two names. WF-000's identity-stack drift (§1) is a disagreement between two or more artifacts about a shared fact — it presumes the fact was captured correctly somewhere, and asks only whether every artifact agrees with it. What Evidence 2 demonstrates has no such presumption: even a project with zero disagreeing artifacts can still lose its own reasoning, if none of its artifacts were ever designed to preserve reasoning in reusable form. WF-001 names this second condition so it can be tested for and closed, the same way WF-000 named drift so it could be tested for and closed:

Operating Model Loss: a state in which a completed project's outcome — what was built — remains documented, while the reasoning that produced it — why specific technical choices were made, what constraints bounded them, how failures were diagnosed and resolved, and what "success" was actually defined to mean — exists only implicitly, scattered across artifacts built for other purposes, none of which was designed to be extracted and reapplied to a different project.

Evidence 1 is not itself an instance of Operating Model Loss — it's an instance of the identity-stack drift WF-000 already governs, and Section 3 resolves it formally using WF-000 §4's own Drift Detection Procedure. It's included here because it demonstrates the more general pattern that motivates urgency: if the plain factual layer of this project is already eroding six weeks after completion, the reasoning layer — which was never even captured as cleanly as the facts were — is at greater and more immediate risk.

Scope

In scope for this section: a working definition of Operating Model Loss, formally distinguished from WF-000's identity-stack drift, and the evidence-based case — two independent, checkable examples — for why WF-001 needs to exist now.

Out of scope for this section (deferred to later sections):

The complete enumeration of what this document will capture — objectives, constraints, architecture, process, metrics, testing, deployment, governance — Sections 4–10
The formal resolution of the Master-CV.md conflict via WF-000 §4's Drift Detection Procedure — Section 3
New vocabulary this document needs beyond Operating Model Loss (Stage, Diagnostic Round, Locked Decision, Generalizable Pattern, and others) — Section 2
The extraction of the reusable template itself — Section 11
Tracking future instances of drift or Operating Model Loss beyond the two cited here — Section 3's evidence base, as it extends over time

Explicitly out of scope for WF-001 as a whole:

Correcting, modifying, or reinterpreting Master-CV.md — a follow-up action for Kingsley outside this workflow, logged as an open item in Section 12
Re-registering the Healthcare Pipeline's GitHub repository, dashboard, or API as identity artifacts — already registered in WF-000 §3, Artifacts 5–7
Narrative resume copywriting, portfolio-site content, or any other execution-level rewriting of a public-facing artifact — belongs to the artifact itself
Outputs

Section 1 establishes the Operating Model Loss definition above. Section 2 formalizes it into the glossary; Section 3 uses it to confirm the Master-CV.md finding classifies as drift rather than Operating Model Loss; Section 11 uses it to confirm the extracted template actually closes the gap it identifies.

Acceptance Criteria
The problem statement is falsifiable against real, current evidence — not a hypothetical — and against two independent examples, not one.
Operating Model Loss is defined precisely enough to be distinguishable from WF-000's identity-stack drift, not a restatement of it under a new name.
The Master-CV.md finding is presented strictly as evidence; it is not corrected, modified, or reinterpreted anywhere in this section.
Nothing in this section promises a capability (e.g., "a reusable template") that a later section doesn't go on to define a mechanism for.
A reader with no prior context on this repository can read this section alone and understand why WF-001 needs to exist.
Verification Checklist
 At least two real, independently-sourced examples are cited and independently checkable against the source artifacts
 Operating Model Loss is defined with an explicit, stated distinction from WF-000's identity-stack drift
 The Master-CV.md finding appears only as cited evidence — no correction, edit, or reinterpretation of Master-CV.md appears anywhere in this section
 Purpose statement makes no unfulfilled promise (the reusable template is named as this document's eventual output, not claimed as delivered here — that is Section 11's job)
 Cross-reference to WF-000 §6.1 is accurate to WF-000 as currently written
 Confirmed by Kingsley before Section 2 begins

Section 1 of 12 complete. Approved 2026-08-06.

2. Scope, Definitions & Governance Inheritance
Purpose

Section 2 does two things Section 1 deliberately deferred: it draws WF-001's document-wide scope boundary, and it supplies the vocabulary the rest of this document needs. Two boundary problems motivate it, mirroring the two that motivated WF-000 §2 for the identity stack. First, without an explicit boundary, "operating model documentation" can expand to include anything loosely related to the pipeline — a live risk for a document whose subject is an already-completed, richly-documented project with no natural stopping point. Second, without a settled vocabulary, later sections would each have to define "pattern," "stage," or "operating model" for themselves, drifting slightly with each use — the same failure mode WF-000 §2 named for "canonical source" and "sync event."

This section also carries the inheritance obligation stated in WF-001's own charter: WF-001 must inherit WF-000's governance principles without duplicating them. Section 2 is where that obligation gets executed — by reference, not by restatement.

Section 1 named the problem: Operating Model Loss, the condition in which a project's reasoning goes uncaptured. This section names what closing that gap actually produces. A completed project generates engineering reasoning — the specific judgment calls, diagnoses, and design choices made along the way, as documented in Sections 4 through 10. Left alone, that reasoning stays bound to the project that produced it. Extracted deliberately — named, stripped of its project-specific detail, and stated in a form that applies elsewhere — it becomes a reusable Engineering Pattern. The set of patterns extracted this way, together with the objectives, architecture, and governance model that produced them, is what this document calls the pipeline's Operating Model: the thing WF-002 through WF-006 actually inherit. Section 11 is where that extraction happens formally; this section only fixes the vocabulary the extraction will use.

Scope

In scope for this section: the document-wide scope boundary for WF-001, and the complete glossary of WF-001-specific vocabulary — including the Operating Model Loss / Engineering Pattern / Operating Model relationship.

Out of scope for this section (deferred to later sections):

The actual extraction of any Engineering Pattern from the pipeline's history — Section 11
Formal application of WF-000's Drift Detection Procedure to the Master-CV.md finding — Section 3
The objectives, architecture, process, metrics, testing, deployment, and governance content itself — Sections 4–10
Outputs
Document Scope Statement

In scope: the healthcare readmission pipeline's objectives, constraints, architecture, engineering process, metrics, testing, deployment, and governance, as evidenced by project_summary.md, presentation_nontechnical.md, and README_portfolio.md — and the Engineering Patterns extracted from that material for reuse by future WF-00X documents.

Out of scope: the pipeline's live deployment endpoints as governed objects (WF-000 §3, Artifacts 5–7 already own that); Master-CV.md as a source of fact (Section 1); any future WF-00X document's own architecture, stack, or subject-matter decisions (WF-000 §6.4 reserves these to that document, and WF-001 has no more claim to them than WF-000 does).

Boundary case, resolved: if a future WF-00X document reuses an Engineering Pattern extracted here and meaningfully modifies it for its own context, the modification is proposed back to WF-001 — the pattern's canonical home — through the ordinary amendment process, rather than forked silently in the downstream document. WF-001 governs a pattern's canonical definition the way WF-000 governs a canonical source for a fact: a downstream document can extend or specialize a pattern's application, but ownership of the pattern's definition doesn't travel with it. This prevents silent divergence: improvements discovered in later workflows strengthen the shared Engineering Pattern rather than creating competing versions of the same idea.

Applied to specific candidates:

Candidate	In scope?	Reasoning
The pipeline's engineering reasoning (Stages 0–9)	Yes	What Sections 4–10 document and Section 11 extracts from
The pipeline's live deployment endpoints (URLs, uptime)	No	Already governed by WF-000 §3, Artifacts 5–7
Correcting Master-CV.md's TP-001 entry	No	Kingsley's action, outside this workflow (Section 1)
A future WF-002 document's own architecture decisions	No	WF-002's own content, per WF-000 §6.4
An Engineering Pattern extracted here, later modified by a downstream document	Boundary case	Canonical definition stays in WF-001; modifications proposed back via amendment process
Glossary of Terms
Operating Model Loss — as defined in Section 1: a state in which a completed project's outcome remains documented while the reasoning that produced it exists only implicitly, scattered across artifacts not designed to preserve it for reuse.
Engineering Pattern — a named, project-independent technique, decision rule, or design choice, extracted from the engineering reasoning behind a specific project and stated in a form intended to apply beyond that project. An Engineering Pattern is the unit Operating Model Loss puts at risk and the unit Section 11 formally extracts. Until it has actually been reused by a second project, its reusability is a design intent, not a confirmed fact — Section 11 states this honestly for each pattern extracted, rather than presenting reusability as already proven. ("Reusable Engineering Pattern" refers to the same term, used where emphasis on reusability is useful.)
Operating Model — the complete, structured account of how a project was run — its objectives, constraints, architecture, engineering process, metrics, testing, deployment, and governance — together with the Engineering Patterns extracted from it. Where an Engineering Pattern is one portable unit, the Operating Model is the aggregate record a downstream WF-00X document actually inherits.
Stage — one of the ten sequential phases (Stage 0 through Stage 9) the healthcare pipeline was built in. Stage 0 covers environment setup; Stages 1 through 9 (referred to in the source material as "the nine stages") cover the substantive pipeline build. Each stage carried its own verification gate before the next began.
Diagnostic Round — one iteration of the detect → investigate → fix → re-verify sequence used in Stage 4 to resolve the model's underperformance, of which there were three. Diagnostic Round is the unit Section 6 examines to extract Engineering Patterns from the pipeline's debugging process specifically, as distinct from its architectural or deployment decisions.
Locked Decision — a governance determination recorded in the pipeline's own docs/00_locked_decisions.md — for example, that approved = TRUE certifies only the Recall ≥ 0.85 floor, not clinical utility. A Locked Decision is Process Artifact-level content in WF-000's sense unless and until WF-001 or a downstream artifact restates its substance as a public claim, at which point WF-000 §4's Drift Detection applies to that restatement.
Governance Table — one of the eight append-only DuckDB tables (ingest_metadata, feature_registry, model_registry, fairness_reports, rag_chunks, rag_index_metadata, llm_call_log, predictions_audit) that record the pipeline's own operational history. Like a Locked Decision, a Governance Table is Process Artifact-level content unless a specific figure from it is restated externally.

A terminology note, not a glossary entry: this document uses "governance" in two senses that share a word but not a referent. WF-000's governance (its Sections 4–5) concerns identity-artifact consistency — drift, canonical sources, approval authority. The pipeline's own governance (Section 10 below; Locked Decision and Governance Table above) concerns the pipeline's internal audit trail and policy checks — a feature of the system being documented, not of this document's own oversight. Where the distinction matters, later sections say which one they mean explicitly.

Inherited From WF-000 (By Reference)

The following apply to WF-001 in full, as already defined in WF-000, and are not restated here: the Section 2 glossary (Identity Artifact, Canonical Source, Sync Event, Drift, Process Artifact, Governing Document); the Section 2 scope boundary, under which WF-001 is itself a Process Artifact unless made public; the Section 3 registry format, for any new public artifact WF-001 might cause to be created; the Section 4 synchronization model in full — trigger taxonomy, Drift Detection Procedure, Verification Workflow, and the four workflow roles; and the Section 5 governance model — versioning scheme (WF-001's own version begins at 0.1, per WF-000 §5.1 and §6.3), approval authority (Kingsley, exclusively), and amendment process.

Per WF-000 §6.4, WF-001 cannot override any of the above, cannot leave a new public artifact unregistered, and cannot bypass Kingsley as approver. WF-001 retains full authority over its own internal technical content — the vocabulary, engineering methodology, extracted Engineering Patterns, and the objectives, architecture, process, metrics, testing, deployment, and governance content of Sections 4 through 10 — none of which WF-000 has, or should have, an opinion about.

Acceptance Criteria
Every term used in Sections 1 and 3–12, as currently planned, is defined here with no forward reference to an undefined term.
Engineering Pattern and Operating Model are each defined precisely enough to be distinguished from each other and from Operating Model Loss — not three words for the same idea.
The Engineering Pattern definition states explicitly that reusability is a design intent until confirmed by actual reuse, rather than asserting it as already proven.
Every element inherited from WF-000 is stated by reference, with a specific section citation, not re-derived or restated in different words.
The document-wide scope boundary can be applied to a new candidate directly, without further interpretation.
Verification Checklist
 Glossary covers Operating Model Loss (by reference to Section 1), Engineering Pattern, Operating Model, Stage, Diagnostic Round, Locked Decision, and Governance Table, plus the governance terminology note
 Engineering Pattern and Operating Model are distinguished from each other explicitly, not left to context
 The Engineering Pattern definition does not claim reusability is proven prior to actual reuse
 Every inherited element cites its specific WF-000 section rather than being restated
 Section 11's title and stub, elsewhere in this document, match the extraction framing established here
 Confirmed by Kingsley before Section 3 begins

Section 2 of 12 complete. Approved 2026-08-06.