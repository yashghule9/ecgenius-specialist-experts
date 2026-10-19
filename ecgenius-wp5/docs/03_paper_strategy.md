# 03. Publication strategy

Goal: publish peer-reviewed papers from WP5 with 2–4 experts first, then extend to all experts. Authors of these docs have **not** done a full novelty search; every novelty statement below is a hypothesis to test in task T15.

## 1. Which experts can carry a paper now?

A paper needs (i) a reference standard, (ii) at least one external test set, and (iii) a claim beyond "we trained a CNN". Based on data that exist publicly or via credentialing:

| Expert | Externally testable on public/credentialed data? | Label tier | Paper-ready? |
|---|---|---|---|
| 5G measurement | Yes: LUDB, QTDB held-out (fiducials) | A (expert fiducials) | Yes, as the enabling component |
| 5B rhythm (AF/flutter first) | Yes: AFDB, CinC2017, CPSC2021, LTAFDB, plus 12-lead sets (PTB-XL, Chapman, CODE-15%) | A/B | **Yes** |
| 5C conduction | Cross-dataset (PTB-XL, Chapman, CODE-15%, MIMIC reports) | B/C | **Yes**, as hybrid measurement+neural |
| 5E structural (low EF, LVH) | EchoNext (restricted) + MIMIC-IV-ECHO linkage [V] as external | A (echo) | **Yes** if credentialing arrives |
| 5F K⁺/QT | MIMIC labs only; no public external site | A (lab) | Internal-only; supports a secondary analysis |
| 5A OMI | Needs partner cath-lab data; MIMIC only proxy (Tier C) | A only via partner | **Flagship, gated** (Paper 2) |
| 5D ischemia/ST-T | No clean labels | weak | Not before Paper 3+ |

**Recommendation:** Paper 1 = 5G + 5B + 5C + 5E (fallback: 5G + 5B + 5C + one of 5E/5F-QT). Paper 2 = 5A OMI when partner data arrive.

## 2. Portfolio

| # | Working title | Core claim (hypothesis) | Experts | Gate | Venue class |
|---|---|---|---|---|---|
| 1 | Measurement-grounded multi-expert ECG interpretation with safety-tier routing and calibrated uncertainty | H1: disagreement between measured (5G rule layer) and neural outputs predicts neural errors beyond ensemble variance. H2: conformal prediction sets keep nominal coverage across datasets and under digitization shift. H3: safety-tier + recall-tuned routing matches "run all experts and fuse" sensitivity at lower latency/cost. | 5G, 5B, 5C, 5E | credentialing (EchoNext), WP4 backbone | npj Digital Medicine / EHJ-Digital Health tier; fallback: CHIL, ML4H, CinC |
| 2 | OMI expert against angiographic outcomes with mimic stress tests | Hybrid neural + criteria expert beats STEMI criteria and machine statement at fixed specificity on unselected chest-pain cohort; false-positive profile on mimics reported | 5A (+5D partial) | hospital partner data (Tier A) | clinical journals (EHJ-DH, JACEP Open, JAMA Netw Open class) |
| 3 | Cost of photographed ECGs: digitization-aware training and refusal rules for Indian paper ECGs | Per-expert drop native→digitized by capture condition; digitization-aware training and confidence-based refusal restore safe operation | all experts, WP2 | Indian paper/photo ECGs | npj DM / Physiol Meas / CinC |
| 4 | Faithful explanations: evidence map + criteria checklist + clinician agreement | Explanations pass in-distribution deletion/insertion tests and reach pre-registered agreement with cardiologist markings | WP8 | WP5 experts, cardiologist time | clinical AI / HCI-health |
| 5 | Guideline-grounded constrained LLM reasoning over structured expert evidence | Verifier-gated generation gives zero unsupported safety-relevant claims on a 500-case set | WP7 | knowledge base; follow TRIPOD-LLM | npj DM / Lancet DH class / ML4H |
| 6 | Full ECGenius external validation + reader study | Study 1–3 of WP9 | all | all | top-tier clinical |

## 3. Novelty status (what the quick search found)

- **Mixture-of-experts/ensembles for ECG exist.** Examples: a multi-expert ensemble for 254 labels (PMC12912303 [R]), a MoE transformer for multi-label diagnosis (ITU thesis [R]), ExChanGeAI sparse-MoE encoder with learned router (arXiv 2503.13570 [P, R]), EnECG ensemble of specialised foundation models (Emory preprint [P, R]). **Do not claim MoE as novel.** Differentiators to defend: (a) *safety tier that cannot be routed away*, (b) router tuned for recall against a "run everything" ablation, (c) arbitration with clinical-measurement criteria, (d) evaluation under dataset and digitization shift.
- **Uncertainty for ECG exists:** six methods compared on 526,656 ECGs, ensembles most robust (Vranken 2021 [R]); conformal risk control for multi-label ECG (Emory preprint [P, R]); Grad-CAM-overlap uncertainty for misclassification detection [R]. **I found no paper testing "fiducial/measured vs deep disagreement as an error signal"** (H1): promising, but run a proper search before claiming.
- **Digitization downstream effect has literature:** printed-PTB scanned-ECG multi-label study (arXiv 2502.14909 [R]), masked training for multi-layout digitized ECGs (arXiv 2508.09165 [P, R]), per-damage-type digitizer robustness (engrXiv [P, R]). Paper 3's novelty must rest on Indian paper/photo data, per-expert drop and an explicit refusal rule, not on "digitization hurts".
- **OMI:** QoH has several external validations (Swedish [S], Portuguese [R], German abstract [R, V]) and a large Swedish deep-learning preprint with culprit localization [P, R]. Paper 2 must add something: Indian-population data, mimic panel, open methodology, or hybrid criteria transparency.

## 4. Paper 1 specification

### 4.1 Research questions
- RQ1: Do specialist experts (with measurement-layer features) generalize across datasets and sites better than a single multi-label head on the same backbone?
- RQ2: Does measured-vs-neural disagreement (H1) improve error detection/selective prediction over ensemble variance and OOD scores?
- RQ3: Do conformal sets retain coverage under dataset shift and digitization shift (H2), and at what set-size cost?
- RQ4: Does safety-tier routing (H3) preserve sensitivity relative to run-all-and-fuse while cutting compute?

### 4.2 Experts, targets, data
| Expert | Targets | Dev data | External data | Notes |
|---|---|---|---|---|
| 5G | P/QRS/T fiducials, PR, QRS, QT/QTc, axis, ST dev | LUDB train folds + PTB-XL+ features | LUDB held-out + QTDB | sampling-rate handling 250/500 Hz |
| 5B | AF/flutter (primary); SVT/VT/pauses only if labels allow | one or two of CPSC2018/Chapman/CODE-15% | other 12-lead sets + AFDB, CinC2017, CPSC2021, LTAFDB (single/dual lead variants) | cross-dataset train-on-A test-on-B grid |
| 5C | 1st-degree AV block, high-grade AV block (if enough positives), LBBB, RBBB, LAFB, WPW exploratory | PTB-XL (+ Chapman) | CODE-15%, MIMIC-IV-ECG report-derived (Tier C) | adjudicate 300-ECG subset with cardiologist |
| 5E | LVEF ≤40% (primary); LVH; moderate-severe AS/MR if sample size allows | EchoNext training split | EchoNext held-out sites; MIMIC-IV-ECHO linkage [V] | per-site recalibration reported |

Tier labels in every results table. If EchoNext access is delayed beyond the Paper 1 start, run Paper 1 with 5G+5B+5C and add 5E as a second-phase extension (or substitute 5F-QT, internal-only, flagged).

### 4.3 Models and baselines
- Backbone from WP4 (default candidates: ECGFounder, LightECGNet v2; check licences [V]).
- Per expert: (a) single multi-label head on the backbone, (b) specialist head (+ 5G features), (c) LightECGNet v2, (d) rule-only measurement baseline, (e) a second public foundation model with linear probe for context.
- Ensembles: 5 seeds per expert; calibration set held out at *dataset/site* level.

### 4.4 Uncertainty and routing
- Signals: ensemble variance, input OOD score (e.g. embedding-distance), digitization confidence (WP2), measured-vs-neural disagreement.
- Evaluate each signal and their combination: AUROC for error detection, risk–coverage curves, AURC, selective accuracy at 90% coverage.
- Conformal: split-conformal and risk-controlled variants; report empirical coverage, set size, per-dataset and per-subgroup coverage; stress with synthetic digitization shift.
- Routing: **Decision for owner:** in Paper 1 the always-on safety tier is limited to what the data support (high-grade AV block, and VT/VF only if a dataset supports it); state clearly if the safety tier is partially simulated.
- Router: gated by triage score, signal quality, age/sex, backbone embedding; ablation vs run-all-and-fuse on sensitivity, latency, FLOPs.

### 4.5 Digitization arm
Render a subset of PTB-XL (or your external set) with ECG-Image-Kit under controlled degradations → run digitizer (own pipeline vs Challenge-winner pipeline) → evaluate experts on reconstructed signals. Report ST/T morphology error, per-expert metric drop, and coverage under shift. If Indian paper ECGs with matched native signals are not yet available, label this arm **synthetic** and promote to real data in Paper 3.

### 4.6 Statistics
Bootstrap (patient-level, ≥1,000 resamples) CIs for AUROC/AUPRC/sensitivity/specificity; DeLong or paired bootstrap for model comparisons; calibration: ECE with bins fixed in the SAP, plus reliability diagrams; subgroup analysis (age, sex, device/dataset, pacing, BBB) with explicit multiplicity handling; PPV reported at stated prevalences; pre-specified primary endpoint per expert and **one** primary headline endpoint for the paper.

### 4.7 Tables and figures (target)
T1 datasets and label tiers; T2 per-expert external performance with CIs; T3 baseline comparison; T4 routing ablation; F1 system diagram; F2 cross-dataset performance grid; F3 reliability + coverage plots; F4 risk–coverage with/without measurement disagreement; F5 digitization degradation curves; F6 failure-case gallery.

### 4.8 Reporting and compliance
- TRIPOD+AI (BMJ 2024;385:e078378) checklist [S/R]; STARD-AI (Nature Medicine 2025;31:3283–3289) if framing as diagnostic accuracy [R, V: confirm pages]; TRIPOD-LLM only for the LLM paper.
- Model card per expert, data statement, code availability statement (code open where licences allow; no redistributing credentialed data), ethics/IRB statement (public/credentialed datasets: confirm exemption language with each data provider).
- Pre-registration (OSF or similar) of RQs, endpoints, SAP before external testing.
- Preprint on medRxiv/arXiv at submission if the target journal allows [V policy per journal].

### 4.9 Risks to Paper 1
| Risk | Mitigation |
|---|---|
| EchoNext credentialing late | Run Paper 1 with 5G+5B+5C; add 5E in revision/extension |
| Conduction labels (Tier B/C) too noisy | 300-ECG adjudicated subset; report noisy-label sensitivity analysis |
| H1 shows no benefit | Report as negative result; paper still stands on RQ1/RQ3/RQ4 |
| Backbone licence restricts publication/commercial use | Check licences first (T0); use permissive backbones for paper |
| Reviewer: "MoE already exists" | Position as safety-tier orchestration + evaluation framework, with the ablations above |

## 5. Authorship, ethics, and integrity
- Agree authorship criteria (ICMJE-style) with the team before drafting; clinicians who adjudicate labels or design operating points should be offered authorship.
- Disclose conflicts, funding and data sources. Comparisons with PMcardio/QoH must rely on published numbers or on tools the owner is licensed to run; do not run proprietary models without permission.
- Claude Code assists with code, analysis and drafting; the authors remain responsible for claims. Disclose AI assistance per journal policy.

## 6. Timeline (aligned with WP5 weeks 6–18)
| Weeks | Paper 1 milestone |
|---|---|
| 1–6 | Novelty search (T15), pre-registration draft, data access, SAP |
| 6–12 | 5G + 5B + 5C experts; harness; first external results |
| 10–16 | 5E; uncertainty + conformal; routing ablation; digitization arm (synthetic) |
| 14–20 | Writing, figures, reporting checklists; internal clinician review |
| 20–24 | Submit; start Paper 2 write-up if partner data are in |

## 7. Venue shortlist (confirm scope, article-type limits, APCs and preprint policy before choosing)
Clinical/AI: npj Digital Medicine; European Heart Journal – Digital Health; Lancet Digital Health (harder); JAMA Network Open; Nature Communications. Technical: Computing in Cardiology; ML4H; CHIL; NeurIPS Datasets & Benchmarks. **Do not rely on this list for deadlines or acceptance fit: [V].**
