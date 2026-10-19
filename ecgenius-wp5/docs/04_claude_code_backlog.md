# 04. Claude Code backlog and repo conventions

## 1. Suggested repo layout (create if the owner has no repo yet; otherwise map onto theirs)
```
ecgenius-wp5/
  docs/                  # these docs (copy of ECGenius_docs)
  configs/               # YAML per experiment (dataset, split, model, seed, thresholds)
  data/                  # gitignored; only manifests + checksums committed
  src/ecgenius/
    datasets/            # loaders: ptbxl, chapman, code15, mimic_ecg, echonext, afdb, cinc2017, cpsc2021, ltafdb, ludb, qtdb
    ontology/            # label maps (versioned), tier tags
    splits/              # patient/site split builders + frozen split files
    measure/             # 5G delineation + measurements + confidence
    experts/             # rhythm, conduction, structural (+ later omi, ischemia, electrolyte)
    uncertainty/         # ensemble, calibration, conformal, OOD, disagreement
    routing/             # safety tier, router, arbitration
    digitization/        # ECG-Image-Kit render + digitizer wrappers
    eval/                # metrics, bootstrap CIs, subgroup, reports
  scripts/               # CLI entry points
  results/               # JSON/CSV with git hash, committed (aggregate only)
  paper/                 # manuscript, figures, tables, checklists
  OPEN_QUESTIONS.md
  DATA_ACCESS_TRACKER.md
```
Tooling defaults: Python, PyTorch, config-driven runs, `pytest`, a lockfile, `ruff`. Keep code small; reuse libraries (wfdb, scikit-learn, numpy/scipy, torch) rather than new frameworks.

## 2. Ordered tasks (each has acceptance criteria)

| ID | Task | Acceptance criteria |
|---|---|---|
| T0 | Scaffold repo, environment, licence audit table for datasets and candidate backbones | `docs/licences.md` lists each dataset/model, licence, commercial-use and redistribution status; unknowns tagged [V] |
| T1 | Data-access tracker | `DATA_ACCESS_TRACKER.md`: PhysioNet credentialing (MIMIC-IV-ECG, MIMIC-IV, MIMIC-IV-ECHO), EchoNext, HEEDB/BDSP, status/owner/date |
| T2 | Ontology and label map v1 | versioned YAML mapping SCP-ECG/SNOMED/dataset labels → 35-node ontology (**ask the owner for the ontology file; do not recreate it**); tier tag per label |
| T3 | Dataset loaders and manifests | each loader returns signal, fs, leads, labels, tier, patient/site IDs; unit tests on shapes, sampling rates, lead order; checksums of manifests |
| T4 | Frozen splits | patient-level splits, site/dataset external sets, split files hashed and committed; leakage test (no patient in two partitions) |
| T5 | Eval harness | AUROC/AUPRC/sens/spec, PPV at prevalence, ECE, bootstrap CIs, subgroup tables, JSON output with git hash; golden tests on toy data |
| T6 | 5G measurement layer | delineation benchmark vs LUDB/QTDB (classical vs DL), per-beat confidence; report ms error; unit tests for QTc formulas |
| T7 | Baselines | LightECGNet v2 (ask owner for code), backbone + linear head, rule-only baseline, all through the harness |
| T8 | 5B rhythm expert | AF/flutter cross-dataset grid, window-length and lead-count ablations; model card |
| T9 | 5C conduction expert | rule layer + neural head, strict vs non-strict LBBB (cardiologist definition), adjudication subset protocol, noisy-label sensitivity analysis |
| T10 | 5E structural expert | EchoNext pipeline (local only), LVEF≤40% primary, per-site recalibration, PPV at screening prevalence |
| T11 | Uncertainty stack | multi-seed ensembles, temperature/isotonic on held-out site, split-conformal (+ risk control), OOD score, measured-vs-neural disagreement, risk–coverage evaluation |
| T12 | Safety tier + router + arbitration | always-on experts, learned router tuned for recall, arbitration table, ablation vs run-all-and-fuse (sensitivity, latency, FLOPs) |
| T13 | Digitization arm | ECG-Image-Kit rendering, digitizer wrappers (own + Challenge winner), per-expert drop curves; label arm synthetic vs real |
| T14 | Paper 1 assembly | tables/figures regenerate from `results/` with one command; TRIPOD+AI checklist filled; manuscript in `paper/` |
| T15 | Literature verification and novelty search | every [R]/[P]/[V] item in docs opened at its primary source; verified bibliography (BibTeX with DOIs); one-page novelty memo for H1–H3; list of mismatches vs these docs |
| T16 | (Gated) OMI expert on partner data | starts only when partner data + label protocol exist (see 02 §5A) |

Suggested order: T0, T1, T15 (start in parallel) → T2–T5 → T6, T7 → T8–T10 → T11–T13 → T14. T16 whenever data arrive.

## 3. Verification protocol for any claim going into a draft
1. Source number from a primary paper you opened (record file + page/table) **or** from `results/*.json` produced by the repo.
2. If a doc here tags it [R]/[P]/[V], re-verify and record the outcome in `docs/verification_log.md` (claim, source, status: confirmed / corrected / unsupported).
3. Never cite a URL you could not open. If an arXiv ID or DOI does not resolve, mark it unresolved instead of guessing.
4. Flag preprints as preprints in the text.

## 4. Decisions the owner must make (put answers in `OPEN_QUESTIONS.md`)
- Final choice of Paper 1 experts (default: 5G+5B+5C+5E) and target venue.
- Where the existing LightECGNet v2, ontology, digitizer and Bayesian engine code lives and what their interfaces are.
- OMI strategy by week 8: partner-data track vs MIMIC proxy (research-only).
- Cardiologist/EP advisors: who, and time budget for the 300-ECG adjudication and operating-point selection.
- Safety-tier scope for Paper 1 (which safety experts are real vs simulated).
- Compute available (GPU type/count) and where credentialed data may be processed.

## 5. What not to do
Do not train on external test sets; do not tune thresholds after seeing external results; do not mix label tiers; do not put raw credentialed data in prompts or commits; do not describe outputs as diagnoses or clinical advice; do not add MoE-novelty claims; do not build a new framework when a library exists.
