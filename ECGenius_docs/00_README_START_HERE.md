# ECGenius docs: START HERE (for Claude Code)

You are joining a research + build project called **ECGenius**: a layered AI system for 12-lead ECG interpretation (photo/scan/PDF/raw signal in; calibrated findings, evidence and guideline-grounded explanation out). The owner needs (a) the **WP5 "Specialist experts" research** done properly and (b) **peer-reviewed publications** from it. Papers can start with 2–4 experts; all experts are built eventually.

## Reading order
1. `01_project_context.md`: whole-project context (architecture, 10 work packages, risks, regulatory frame).
2. `02_WP5_experts_plan.md`: per-expert research plan (5A–5G), evidence base, shared protocol.
3. `03_paper_strategy.md`: publication portfolio, **Paper 1 spec**, venues, reporting standards, novelty status.
4. `04_claude_code_backlog.md`: repo layout, ordered tasks with acceptance criteria, verification protocol.

## Non-negotiable working rules
1. **Never invent numbers, citations, DOIs, or author lists.** Every number in a draft must trace to a primary source you opened, or to code output in this repo. Evidence tags used in these docs: **[S]** source read in full, **[R]** seen only in search results/abstract, **[P]** preprint, **[V]** must verify against primary source before use. Task T15 converts [R]/[P]/[V] items into verified citations. Until then, treat them as leads.
2. **Credentialed data stays local.** MIMIC-IV-ECG, MIMIC-IV, MIMIC-IV-ECHO, EchoNext and HEEDB have data-use agreements. Do not upload raw records or derived row-level data to any hosted LLM/API/cloud notebook, and do not commit them to git. PhysioNet has a stated policy on using credentialed data with online LLM services **[V: read the current policy]**. Write code that runs on the owner's machine; commit only code, configs, aggregate results and synthetic examples.
3. **Safety design rule (from the project):** the orchestrator must never silently decide *not* to look for a dangerous condition. Safety experts run on every ECG; routing only chooses *additional* experts. An LLM never creates, upgrades or downgrades a safety alert.
4. **Label tiers:** A = angiography/echo/lab-confirmed; B = cardiologist consensus; C = machine- or report-derived. Store the tier on every label; never pool tiers silently.
5. **Splits are patient-level and frozen before modelling.** Each expert needs at least one dataset-level external test set. No tuning on external sets. Thresholds are set on development data (or a held-out site) before external testing.
6. **Report honestly,** including when external performance is lower than internal. Negative results go into the paper.
7. **Pre-register** the primary hypotheses and analysis plan (OSF or similar) before running external tests. Log deviations.
8. **Reproducibility:** fixed seeds (multi-seed ensembles), pinned environments, config-driven runs, results written to versioned JSON/CSV with git hash, data-card for each dataset.
9. **Not a medical device and not clinical advice.** Everything here is research. Do not write language implying clinical use. Regulatory context (India CDSCO) is in `01_project_context.md`.
10. When unsure about a clinical definition (e.g. strict LBBB criteria, OMI definition, QTc formula) stop and put the question in `OPEN_QUESTIONS.md` for the cardiologist advisor; do not guess.

## Owner context
- Based in India; Indian-population validation is a stated differentiator (a hospital partnership is being pursued; none confirmed in these docs).
- Existing assets mentioned in the plan: a LightECGNet v2 baseline model, a 35-node ontology, a Bayesian reasoning engine, a SHAP/ablation panel, a U-Net/perspective/TPS digitizer pipeline, RBAC. Their code is **not** in these docs; ask the owner where it lives before assuming anything about its interface.
- Team: about 4–6 people, plus cardiologist and emergency-physician advisors (to be recruited/confirmed).

## Glossary
OMI = occlusion myocardial infarction (acute coronary occlusion, with or without ST elevation). STEMI = ST-elevation MI by millimetre criteria. QoH = Queen of Hearts, PMcardio's OMI model (the reference design/comparator). OOD = out-of-distribution. ECE = expected calibration error. PPV = positive predictive value. SCP-ECG = standard communications protocol statements used by PTB-XL. TRIPOD+AI / STARD-AI = reporting guidelines for AI prediction / diagnostic-accuracy studies.
