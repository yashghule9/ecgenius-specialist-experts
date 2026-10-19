# 01. Project context (condensed from "ECGenius: Multi-Expert Research and Build Plan")

Source document: the owner's 13-page plan (dated after Sept 2026). Facts below are the plan's own claims; those marked [V] in the plan still need primary-source checks.

## 1. Executive summary
Move ECGenius from one 45-label classifier to a **layered system**: digitizer + quality gate → shared backbone → orchestrator → specialist experts (always-on safety tier + routed tier) → fusion/calibration/uncertainty → reasoning (ontology + Bayesian core, guideline RAG, constrained LLM) → explanation + clinician UI → audit/feedback/monitoring.

Four findings shaped the plan:
1. **PMcardio is not one model plus an LLM.** Flagship Queen of Hearts (QoH) is a dedicated network for OMI trained against angiographic outcomes (>18,000 ECGs with angiography reference, per the plan), with 50+ findings overall; explanations are heatmaps (ECGxplain) over 2.5 s segments, not LLM text.
2. **External validation is where claims break.** QoH: 80.6% sensitivity / 93.7% specificity in original validation; 52% / 99% in 24,513 unselected Swedish ED visits [S: read]; 58.9% / 85.7% in a Portuguese AMI cohort [R]. Case mix and reference standard drive the difference.
3. **Bigger backbones do not automatically win.** A 2026 ICLR benchmark (26 tasks) found several transformer SSL models did not beat a strong supervised S4 baseline; CNN-based supervised ECGFounder was consistently among leaders [R, P]. Choose the backbone by own benchmark.
4. **India has a final software-as-medical-device rulebook:** CDSCO guidance doc CDSCO/MD/GD/MDSW/01/2026 (21 July 2026) [V]. Disease screening/clinical decision-support software is likely regulated; "decision support" wording alone does not exempt.

**Core design rule:** safety experts run on every ECG; routing decides only which *additional* experts run.

## 2. Architecture table
| Layer | Role | Key rule |
|---|---|---|
| Digitizer | image → calibrated 12-lead signal | report per-lead SNR, never hide failures |
| Quality gate | reject/warn on poor input | show reason to user |
| Backbone | shared encoder | chosen by benchmark |
| Safety tier (always on) | OMI/STEMI, VT/VF, high-grade AV block, severe hyperkalemia pattern, long QT | every ECG, high sensitivity, overrides routing |
| Routed tier | rhythm, conduction, hypertrophy, structural, etc. | by triage score + clinical context |
| Fusion | combine, calibrate, detect OOD | one output with explicit uncertainty flag |
| Reasoning | ontology, Bayesian evidence, guideline RAG | LLM explains and cites; never changes safety tier |
| Explanation | heatmaps, criteria checklist, measurements | validated for faithfulness and clinician agreement |

## 3. The ten work packages (one paragraph each)
- **WP1 Data/labels/ontology.** Datasets: PTB-XL (21,799 ECGs, 18,869 pts, 71 SCP-ECG statements), MIMIC-IV-ECG (~800k, ~160k pts, credentialed), CODE-15% (345,779), Chapman-Shaoxing (45,152), HEEDB (>11M, credentialed via BDSP), EchoNext (ECG+echo, restricted). One ontology map (SCP-ECG, SNOMED CT → 35-node ontology), label tiers A/B/C, patient-level splits, hold out whole sites as external tests, start Indian data partnership early. Exit: two external test sets frozen.
- **WP2 Digitization.** PhysioNet Challenge 2024 (ECG-Image-Kit, ECG-Image-Database 35,595 images from 1,977 records); winner = Hough transform + nnU-Net (code: felixkrones/ECG-Digitiser). Phone photos much harder (one entry ~ -9 dB SNR). Benchmark own pipeline; synthetic Indian layouts; digitization-aware training; per-lead confidence. Exit: documented accuracy loss per expert and a rule for when to refuse a photo.
- **WP3 Quality gate and triage.** Signal-quality, lead-reversal, paper/photo quality, fast normal/abnormal/possibly-critical triage tuned for critical sensitivity, age/sex/symptom handling. Exit: critical-finding sensitivity target on external data.
- **WP4 Backbone selection.** Bake-off: LightECGNet v2, ECGFounder (CNN, ~33.8M), ECG-FM (~94M), ECG-JEPA, ST-MEM (~90M), HuBERT-ECG (~97M), MERL, ECG-CPC, S4/ResNet baseline; frozen vs fine-tuned; check licences. Exit: decision backed by external-set numbers.
- **WP5 Specialist experts (core research).** See `02_WP5_experts_plan.md`.
- **WP6 Orchestrator, fusion, calibration.** Always-on safety tier; learned router tuned for recall; arbitration (more serious finding shown, conflict flagged, human review requested); temperature/isotonic + conformal prediction; uncertainty from ensemble disagreement + OOD + digitization confidence + measured-vs-neural disagreement; ablate against "run everything and fuse". Exit: no critical finding missed because of routing on external set.
- **WP7 Knowledge and LLM reasoning.** Guideline corpus (ESC, AHA/ACC, Universal MI definition, ISHNE/AHA ECG standards, CSI), structured evidence JSON, hybrid retrieval, constrained generation with schema (finding, evidence ID, guideline passage ID, strength), verifier removes unsupported claims, hard rules (LLM cannot change safety tier), adaptive questioning. Related systems to read: ECG-Chat, CARE-ECG, Q-Heart, ECG-LM, PULSE, Zodiac, CardAIc-Agents.
- **WP8 Explainability.** Four layers: evidence map (lead-by-time attribution), criteria checklist (deterministic, from measurements), measurement overlay (fiducials), rationale + counterfactual. Faithfulness (deletion/insertion with in-distribution replacement, e.g. patient's median beat), clinician agreement (300 ECGs), usability study.
- **WP9 Validation and clinical evidence.** Study 1 unselected retrospective multi-site external validation; Study 2 digitized-pathway validation (Indian photo/scan); Study 3 reader study with/without ECGenius; subgroups; missed-critical review; comparators (machine algorithm, STEMI criteria, cardiologists, a general LLM); reporting STARD-AI and TRIPOD+AI.
- **WP10 Production, MLOps, security, regulatory.** ONNX/TensorRT, p95 latency target, model registry, drift monitoring, HITL feedback, DPDP Act alignment [V], CDSCO MDSW pathway (risk classes A–D, Algorithm Change Protocol), ISO 13485/14971, IEC 62304/82304-1/81001-5-1 [V which are named], regulatory consultant to confirm class; PMcardio reference path: CE Class IIb, FDA Breakthrough (Mar 2025), De Novo announced Sept 2026 (company statements; confirm in FDA database).

## 4. Timeline (about 7 months, 4–6 people)
A Foundations wk 1–8 (WP1–4) · B Experts wk 6–18 (WP5–6) · C Reasoning/explanation wk 10–22 (WP7–8; may use mocked expert outputs) · D Validation/production wk 18–30 (WP9–10). Critical path: data-access approvals and Indian hospital partnership (outside your control; start week 1).

## 5. Risk register (plan's own)
No angiography-linked OMI labels · external performance drop · digitization corrupts ST segments · LLM hallucination · router misses a condition · misleading heatmaps · dataset licences limit commercial use · regulatory class underestimated · label noise in public data.

## 6. Caveats from the plan itself
PMcardio blog/press figures are claims until primary papers are read; several 2026 preprints are not peer reviewed; FDA details come from company statements and a secondary newsletter; items tagged [V] need primary-source checks.

## 7. Source list (from the plan)
PMcardio/QoH: powerfulmedical.com (product, FAQ, research pages); Swedish ED validation (JACEP Open, PMC13482599); Portuguese validation (EHJ-DH supplement); inflammatory myopericardial syndromes (ScienceDirect S2772963X26004679). Digitization: CinC 2024 overview (cinc.org/archives/2024/pdf/CinC2024-011.pdf), github.com/felixkrones/ECG-Digitiser, ECG-Image-Kit arXiv 2307.01946, ECG-Image-Database arXiv 2409.16612, npj Digital Medicine s41746-025-02327-1. Foundation models/datasets: ICLR 2026 benchmark (arXiv 2509.25095), FOUND-AF (arXiv 2608.03597), "How do ECG models scale" (arXiv 2605.17276), OpenECG (arXiv 2503.00711), ECG dataset catalogues (github aaekay/ecg-datasets, vlbthambawita/ECGDatasets), PTB-XL (physionet.org/content/ptb-xl/1.0.3). LLM: arXiv 2607.20814, NeurIPS 2025 poster 132675, CARE-ECG arXiv 2604.10420, ECG-Chat arXiv 2408.08849, Q-Heart arXiv 2505.06296, review arXiv 2604.02501. Explainability: arXiv 2508.17294, ESC journals 90108 and 90090, saliency pitfalls arXiv 2609.25978. Regulatory: natlawreview, meddeviceguide, medicalbuyer coverage of CDSCO MDSW guidance. (Many are 2026 preprints; verify each resolves and check whether a peer-reviewed version exists before citing.)
