# WP5 Specialist Experts: Research Plan (v0.1)

Scope: expands WP5 (5A–5G) of the ECGenius Multi-Expert Research and Build Plan. Window: weeks 6–18, feeding WP6.
Evidence status tags: **[S]** source page read; **[R]** seen only in search results/abstract; **[P]** preprint; **[V]** needs primary-source check before you quote it.

---

## 1. Bottom line

1. **Most feasible on public data now:** 5G (measurement layer), 5B (rhythm), 5C (conduction), 5F-QT/K⁺ (MIMIC labs), 5E (EchoNext, restricted but available).
2. **Gated on data you do not have yet:** 5A (OMI). Public data cannot give angiography-grade labels. MIMIC can only give a *proxy* (ACS + PCI + troponin). Plan two tracks and call the MIMIC one research-grade.
3. **The OMI benchmark to beat is not "STEMI criteria".** In the only large unselected external study I could read, QoH got 52% sensitivity / 99% specificity / 51% PPV at a 1.9% OMI prevalence (24,511 chest-pain patients, Sweden). STEMI criteria got 23% sensitivity. Your exit criterion should be stated against *that kind* of cohort and *those* comparators.
4. **Alert burden decides the safety tier.** At 90% sensitivity / 90% specificity and 2% prevalence, PPV is about 15%. For hyperkalemia (Mayo model: ~90% sensitivity but only 55–63% specificity at that point), PPV at low prevalence is far lower. Safety-tier experts need a defined *alert type* ("consider K⁺ check") and an alert-rate budget, not only a sensitivity target.
5. **Two changes to your exit criteria are needed** (see section 6): "beats LightECGNet v2 on external data" is not testable for 5A, 5E and 5F without external data that does not exist publicly; and the baselines must include the clinical comparators, not only your own model.

---

## 2. Feasibility map (what the literature supports)

| Expert | Strongest evidence found | Public data you can use now | Reference standard available? | Feasibility | Main risk |
|---|---|---|---|---|---|
| 5A OMI/STEMI | QoH: Swedish ED cohort 52%/99% [S]; Portuguese AMI cohort 58.9% sens / 85.7% spec (abstract) [R]; German CPU >1,700 pts, 95.3% acc, vendor-hosted [R, V]; large Swedish deep-learning preprint, 540,372 ECGs linked to cath outcomes, C-stat >0.95, culprit localization (LM/LAD, LCX, RCA) [P, R] | MIMIC-IV-ECG + MIMIC-IV hosp/ED (credentialed) | **Proxy only** in MIMIC; real labels need a cath-lab partner or registry | Gated | Label quality; external drop-off |
| 5B Rhythm | FOUND-AF: ECGFounder strongest across AFDB, CinC2017, CPSC2021, LTAFDB [P, R] | AFDB, CinC2017, CPSC2021, LTAFDB, Leipzig Heart Center (SVT, VT, pre-excitation, pacing) [R], VitalDB arrhythmia [R] | Yes (annotated rhythm) | High | Dataset shift between Holter and 12-lead; pause detection evidence not found |
| 5C Conduction | Mostly small single-site studies; LBBB model 10,344 ECGs, no external test [R] | PTB-XL conduction statements, Chapman, CODE-15%, LUDB (delineation) | Cardiologist/machine labels (tier B/C); measurable (PR, QRS) | High for hybrid design | Label noise; LBBB strict vs non-strict; AV block under-represented |
| 5D Ischemia/ST-T | Little on NSTEMI/subendocardial/Wellens/de Winter as separate labels (none found) [R]; pericarditis DL 2022: 128 pericarditis ECGs [R] | PTB-XL, MIMIC (weak) | Weak | Low–medium | No clean labels; tiny rare-pattern sets |
| 5E Structural | EchoNext (Nature 2025): 1.2M pairs, 230k pts training; 77% vs 64% cardiologist sensitivity (press summary) [R]; amyloid AUC 0.95, HCM 0.92, AS 0.90, hypertensive LVH 0.76 [R]; HCM external cohorts Bern/Oxford/Seoul [R] | EchoNext PhysioNet v1.0.0 (82,543 ECG–echo pairs, 36,286 pts, restricted, 250 Hz) [R]; MIMIC-IV-ECHO (credentialed) [R, V] | **Yes (echo)** | Medium–high | Prevalence shift: Mayo LVEF model had 26.9% sens at original cutoff in a 0.6%-prevalence population (AUROC 0.82) [R] |
| 5F Electrolyte/QT | K⁺: Mayo JAMA Cardiol, AUC 0.883/0.860/0.853 at three sites, ~90% sens, spec 63/55/55% [R]; single-lead preprint AUROC 0.861 external for K⁺ ≥6.0 [P, R]. LQTS: Amsterdam/Leuven AUC 0.89–0.92 [R]; multi-site HiRO CNN beat expert QTc incl. concealed LQTS (JAMA Cardiol 2024) [R] | MIMIC-IV (labevents ↔ ECG) | K⁺: lab value (good if timed tightly). LQTS: genotype, scarce | K⁺ medium; LQTS medium; Brugada low | Time-gap noise; low specificity; tiny positive sets |
| 5G Measurement | LUDB (200 × 10 s, 12-lead, 500 Hz, ODC-By) [R]; QTDB (2-lead, 250 Hz, sparse labels) [R]; DL delineation with external QTDB/LUDB tests [R] | LUDB, QTDB | Yes (expert fiducials) | High | Photo-derived signals distort ST/T amplitude |

---

## 3. Shared protocol for all experts

Define once, apply to every expert. It is the core of WP5's scientific credibility.

1. **Label card per expert:** source, tier (A/B/C as in WP1), time window between ECG and reference, exclusions. Never pool tiers silently.
2. **Splits:** patient-level; at least one *site-level* and one *dataset-level* external test set per expert; frozen before modelling.
3. **Baselines (every expert):** (a) LightECGNet v2, (b) best frozen backbone from WP4 + linear head, (c) a classical-measurement rule baseline, (d) where relevant a clinical comparator (STEMI criteria, Glasgow/MUSE statement, QTc cutoff).
4. **Hybrid design:** neural head receives backbone embedding *and* 5G measurements; report ablation with and without measurements.
5. **Calibration:** per-expert temperature/isotonic on a held-out *site*, then conformal sets; report ECE per subgroup and per site.
6. **Operating points set before external testing:** safety tier at a fixed high sensitivity (target agreed with cardiologist), balanced elsewhere. Report PPV at realistic prevalence and *alerts per 1,000 ECGs*.
7. **OOD and uncertainty:** multi-seed ensemble (ensembles were the most robust method across 526,656 ECGs in a six-method comparison, Vranken 2021) [R]; conformal risk control for multi-label sets is an active line (Emory preprint) [P, R]; add input-distribution OOD score.
8. **Digitized-signal arm:** evaluate every expert on native and on digitized-reconstructed signals (links to WP2).
9. **Model card + failure library:** subgroup results (age, sex, device, pacing, BBB), top failure modes with example ECGs.

---

## 4. Expert sub-plans

### 5G. Classical measurement layer (build first; everything else depends on it)
- **Objective:** P/QRS/T delineation → PR, QRS, QT/QTc, axis, ST deviation, voltage criteria, with a per-measurement confidence.
- **Method:** (1) benchmark one classical delineator (wavelet-based, e.g. NeuroKit2/ECGdeli [V: confirm licences and accuracy]) against a U-Net-style segmenter on LUDB and QTDB; (2) fine-tune the segmenter on LUDB + PTB-XL+ median-beat features; (3) sampling-rate handling (QTDB is 250 Hz, LUDB 500 Hz).
- **Experiments:** fiducial error (ms) vs expert annotation; QTc agreement across formulas (Bazett, Fridericia); ST-level error in µV on native vs digitized signals; failure under noise and pacing.
- **Exit:** measurement error within a pre-agreed tolerance on LUDB external; ST-deviation error reported by capture condition; confidence flag that correctly marks unreliable beats.
- **Effort:** 4–5 weeks, 1 signal-processing engineer.

### 5A. OMI / STEMI-equivalent expert (highest priority; gated)
- **Hard facts to design around**
  - QoH, same model, different cohorts: 80.6% sens/93.7% spec (original, from your plan) vs 52%/99% (Swedish ED, 1.9% prevalence) vs 58.9%/85.7% (Portuguese AMI cohort). Case mix and reference standard drive the numbers.
  - Swedish study reference: angiography with culprit lesion TIMI 0/1, or hs-TnT ≥1000 ng/L in highly suspected ACS when no angiography. Retrospective; authors have QoH-company ties. [S]
  - QoH specificity in inflammatory myopericardial syndromes: 97.7% in (myo)pericarditis, 87.7% in (peri)myocarditis (JACC Advances 2026) [R]. This is the mimic-test template.
  - Pericarditis-vs-STEMI DL (2022) used only 128 pericarditis ECGs [R]; mimic sets are small everywhere.
- **Two label tracks (run in parallel)**
  - **Track 1, strategic:** hospital partner with cath-lab records: culprit-occlusion label (TIMI 0/1 or equivalent), troponin trajectory, door-to-cath times. Target ≥1–2k OMI cases plus consecutive chest-pain controls from ≥2 sites. Tier A.
  - **Track 2, MIMIC proxy:** ECG within a fixed window before a PCI procedure code plus troponin trajectory, vs controls. Feasibility of linking is **[V]**; ICD procedure codes do not tell you the culprit was occluded, so label this Tier C and use it for pretraining, mimic mining and pipeline development, never for headline claims.
- **Model:** backbone + 5G measurements + sub-heads: OMI (primary); STEMI-equivalent patterns (de Winter, Wellens, posterior MI, hyperacute T) as auxiliary heads trained only where labels exist; lead-wise evidence maps for WP8. Output categories modelled on QoH: STEMI-equivalent, high-risk NSTEMI, subendocardial ischemia [from your plan].
- **Experiments**
  1. Sensitivity at fixed specificity (95%, 99%) vs STEMI criteria, extended criteria and machine statement on the *same* cohort.
  2. PPV at prevalence 1–5% and alerts per 1,000 ECGs.
  3. Mimic panel: early repolarization, LVH, pericarditis, LBBB, Brugada pattern, paced rhythms (false-positive rate per mimic).
  4. Non-STEMI occlusion subgroup; localization (LM/LAD, LCX, RCA) if Track 1 labels allow.
  5. Door-to-decision simulation; serial-ECG analysis if available.
  6. Digitized-signal arm.
- **Exit (proposed):** on unselected chest-pain external data, sensitivity above conventional STEMI criteria by a pre-registered margin at ≥95% specificity, with CI; mimic false-positive rates reported; model card.
- **Decision needed:** partner-or-proxy by week 8; if no partner by then, ship 5A as "research use only" with Track 2.

### 5B. Rhythm expert (AF, flutter, SVT, VT/VF, pauses)
- **Data:** AFDB, CinC2017 (30–60 s single-lead; AF-vs-normal subset is 30,825 ten-second windows at 12.8% AF in FOUND-AF), CPSC2021, LTAFDB (≈1.41M ten-second segments), Leipzig Heart Center (SVT/VT/pre-excitation), VitalDB arrhythmia; MIT-BIH malignant ventricular ectopy for VT/VF. Pauses: **no benchmark found in my search**; define a Holter-based pause label with a cardiologist [V].
- **Method:** three-way comparison on frozen embeddings: ECGFounder (best on AF in FOUND-AF [P]), your backbone, a long-window model with RR-interval features. Two-stage for VT/VF: short-window safety detector (high sensitivity) plus rhythm-strip confirmation.
- **Experiments:** cross-dataset AF (train on one, test on others), 10 s vs 30 s vs 60 s windows, lead-count ablation (12-lead, lead I, lead II), paroxysmal AF onset/offset error, flutter vs AF confusion, VT vs SVT-with-aberrancy, rate-dependent performance.
- **Safety-tier rule:** VT/VF always on, thresholded for sensitivity; report false alerts per hour on Holter.
- **Exit:** external-dataset AUROC and sensitivity at a fixed specificity per rhythm class; no cross-dataset collapse >pre-set delta.

### 5C. Conduction expert (AV blocks, BBB, fascicular, pre-excitation)
- **Evidence gap:** I found no study with external validation of DL specifically for AV block/BBB detection [R]; LBBB model on 10,344 ECGs had only cross-validation [R]. So this expert is mostly a **measurement-first** design, which is what your plan already proposes.
- **Method:** rule layer from 5G (PR, QRS, morphology criteria; strict LBBB criteria such as Strauss [V]) + small neural head for morphology; 2nd/3rd degree block needs multi-beat context (P:QRS relationship), not a 10 s average beat.
- **Pre-excitation/WPW:** small pediatric 12-lead DL (CinC 2025) and lead-I preprint exist [R]; treat as exploratory, delta-wave from 5G median beat as the interpretable feature.
- **Experiments:** PR/QRS agreement vs manual; LBBB strict vs non-strict; AV-block recall by degree; behaviour with pacing; label-noise audit on a 300-ECG cardiologist-adjudicated subset.
- **Exit:** high-grade AV block sensitivity target (safety tier) on external data; BBB per-class F1; rule-vs-neural disagreement flagged.

### 5D. Ischemia and ST-T expert (non-occlusive ischemia, subendocardial, pericarditis pattern)
- **Evidence:** weakest in the literature (no clean NSTEMI/subendocardial/Wellens/de Winter label sets found) [R]. PTB-XL MI work exists but mixes acute/chronic MI [R].
- **Recommendation:** do **not** promise a standalone classifier at week 18. Deliver (i) lead-wise ST/T measurement features from 5G; (ii) a pericarditis-pattern and early-repolarization detector trained from the MIMIC/PTB-XL mimic mining done in 5A; (iii) a "high-risk NSTEMI" category *only* if Track 1 data exist.
- **Experiments:** pericarditis-vs-OMI discrimination using 5A mimic panel; ST-deviation agreement with cardiologists; ablation of measured vs neural.
- **Exit (reduced):** pericarditis-pattern and early-repolarization detection with CI on external data; ischemia output labelled "pattern" not "diagnosis".

### 5E. Structural expert (LVH, low LVEF, valvular, cardiomyopathy screening)
- **Data:** EchoNext (restricted, 250 Hz, one-year ECG–echo window); MIMIC-IV-ECHO linkage [V]. Indian echo-linked data from your partner for external test.
- **Targets (separate heads):** LVEF ≤40%, LVH/wall thickness, moderate-severe AS/MR (valvular), cardiomyopathy flag (HCM, amyloid) where positives suffice.
- **Key lesson:** discrimination transfers, **calibration and cutoff do not**. Mayo LVEF model: AUROC 0.82 but sensitivity 26.9% at the original cutoff in a population with 0.6% prevalence [R]. Model trained at one hospital generalized worse at another (Yagi 2022) [R]. Plan site-specific recalibration and report PPV at screening prevalence.
- **Experiments:** per-site recalibration; sampling-rate robustness (250 vs 500 Hz); echo-ECG time-gap sensitivity (0–30 d vs 1 yr); amyloid/HCM mimic specificity among LVH and AS patients; digitized arm.
- **Output rule:** always "screening, echo advised" (from your plan).
- **Exit:** external AUROC and PPV at stated prevalence, per target; calibration error per site.

### 5F. Electrolyte, QT and channelopathy expert
- **Hyperkalemia (safety tier, per your plan):** Mayo multi-site ~90% sensitivity at 55–63% specificity, NPV >99% [R]; single-lead preprint AUROC 0.861 for K⁺ ≥6.0 [P, R]. Labels from MIMIC labevents within a tight window (e.g. ≤1–2 h, [V]); exclude values drawn after treatment.
  - *Design implication:* use as a **rule-out / "consider K⁺ test" alert** with explicit alert-rate budget; consider a two-threshold scheme (moderate ≥5.5, severe ≥6.0).
- **QT / long QT:** hybrid = measured QTc (5G) + neural score. Published models beat manual QTc in concealed LQTS (multi-site HiRO) [R]; genotype-confirmed positives are scarce, so partner registries are required for external testing.
- **Brugada:** PNAS-family 2023 paper (AUC 0.934 [V: journal]) and a public Brugada-HUCA dataset [R]; small data, treat as exploratory with explicit "pattern" language.
- **Experiments:** performance vs lab-ECG time gap; CKD/dialysis subgroup; QTc by formula and rate; hypokalemia as a separate head only if data volume allows.
- **Exit:** hyperkalemia sensitivity at fixed alert rate on an external site; QTc agreement and LQTS detection vs expert QTc on any external registry available.

---

## 5. Timeline (weeks 6–18, aligned with phase B)

| Weeks | Work | Gate |
|---|---|---|
| 6–8 | Shared harness (splits, metrics, calibration, model-card template); 5G v1; OMI partner-or-proxy decision | Decision logged week 8 |
| 8–12 | 5A Track 2 baseline; 5B AF/rhythm benchmark; 5C rule layer | First OMI vs comparators table |
| 10–15 | 5F (K⁺, QTc); 5C neural head; 5A Track 1 if data arrived | Calibration report per expert |
| 12–18 | 5E on EchoNext + Indian external; 5D reduced scope; digitized arm for all | Locked thresholds and model cards, input to WP6 |

Dependencies: WP1 credentialing (MIMIC-IV-ECG, EchoNext), WP4 backbone choice (until then use ECGFounder + LightECGNet v2 in parallel), WP2 digitized signals.

---

## 6. Suggested edits to the WP5 text

1. "Each expert beats LightECGNet v2 on external data": add *and* the relevant clinical comparator, and name the external set per expert up front. For 5A/5E/5F, say "external = partner or held-out public site".
2. Add an **alerts per 1,000 ECGs** budget to the safety tier (OMI, hyperkalemia).
3. Label 5D explicitly as exploratory; split "ischemia" claims from "pattern" outputs.
4. Add pause detection to 5B only with a defined label source.
5. Add sampling rate (250/500 Hz) harmonization to WP1/WP5 (EchoNext is 250 Hz).

---

## 7. Open questions for you

- Which Indian hospital(s) can give cath-lab-linked ECGs, and in what form (paper, photo, raw)?
- Do you accept shipping 5A as research-only if no Tier A data by week 8?
- Cardiologist time available for the 300-ECG adjudication and for choosing safety-tier sensitivity targets?
- Is hyperkalemia allowed to alert at low specificity, or should it be an advisory only?

---

## 8. Sources (grouped)

- QoH Swedish ED validation (JACEP Open 2026): https://pmc.ncbi.nlm.nih.gov/articles/PMC13482599/ [S]
- QoH Portuguese cohort (abstract): https://academic.oup.com/ehjdh/article/7/Supplement_1/ztaf143.130/8422905 [R]
- QoH German CPU abstract (vendor-hosted): https://www.powerfulmedical.com/research/artificial-intelligence-based-detection-of-occlusion-myocardial-infarction-first-external-validation-in-a-german-chest-pain-unit-cohort/ [R]
- Swedish OMI localization preprint: https://www.medrxiv.org/content/10.1101/2025.09.11.25335407v1.full [P, R]
- Inflammatory myopericardial syndromes (QoH): https://www.sciencedirect.com/science/article/pii/S2772963X26004679 [R]
- Acute pericarditis DL: https://pmc.ncbi.nlm.nih.gov/articles/PMC9324403 [R]
- FOUND-AF: https://arxiv.org/html/2608.03597 [P, R]
- EchoNext (Nature 2025, DOI 10.1038/s41586-025-09227-0); PhysioNet EchoNext dataset [R]; MIMIC-IV-ECHO: https://www.physionet.org/content/mimic-iv-echo/ [R]
- LVEF external validation (Attia 2021): https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7955278/ ; multi-institution LVEF (Yagi 2022): https://pmc.ncbi.nlm.nih.gov/articles/PMC9779862 [R]
- LVH etiology models: https://pmc.ncbi.nlm.nih.gov/articles/PMC10123506/ ; HCM external: https://boris.unibe.ch/199415/1/ztae029.pdf [R]
- Hyperkalemia (Mayo, JAMA Cardiol 2019) and single-lead preprint: https://arxiv.org/pdf/2603.14177 [R, P]
- LQTS: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9063181/ ; JAMA Cardiol 2024: https://jamanetwork.com/journals/jamacardiology/article-abstract/2815659 [R]
- Brugada: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10627411/ ; WPW preprint: https://www.medrxiv.org/content/10.1101/2024.09.26.24314432.full.pdf [R]
- Delineation: LUDB https://physionet.org/content/ludb/ ; DL segmentation https://arxiv.org/pdf/2304.06237 [R]
- Uncertainty: Vranken et al. 2021 (UMC Utrecht) ; Emory conformal risk control preprint https://cs.emory.edu/~jyang71/files/ea-crc.pdf [R, P]
- Leipzig Heart Center ECG: https://www.physionet.org/content/leipzig-heart-center-ecg/ ; VitalDB arrhythmia: https://www.physionet.org/content/vitaldb-arrhythmia/ [R]

**Limits of this review:** one article read in full; most others from search summaries. Search was US-only and may miss Indian or non-English work (I found no public Indian 12-lead corpus, which may be a search gap). Every [R] and [P] item needs primary-paper reading before any number goes in a dossier.
