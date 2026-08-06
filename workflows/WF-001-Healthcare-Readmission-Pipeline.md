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