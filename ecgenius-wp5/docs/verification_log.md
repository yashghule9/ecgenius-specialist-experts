# Literature verification log (T15)

Every [R]/[P]/[V] item from `01_project_context.md`, `02_WP5_experts_plan.md`, `03_paper_strategy.md` checked against its primary source on 2026-10-19 (four parallel passes; some sources were blocked for direct fetch and corroborated via independent secondary coverage instead — noted per row). Status values: **confirmed** (number/claim matches source), **corrected** (source found, number/attribution differs from the doc), **unsupported** (source found, claim not in it), **unresolved** (source could not be located/opened at all).

## OMI / QoH (5A, 01§2, 02§8)

| Claim | Source | Status | Detail |
|---|---|---|---|
| QoH Portuguese cohort 58.9%/85.7% | academic.oup.com ztaf143.130 | **Corrected** | Numbers match exactly (n=812) but the model tested is named **PMcardio-OMI** in the abstract, not "QoH" — check whether these docs are conflating two Powerful Medical products |
| QoH German CPU cohort >1,700 pts, 95.3% accuracy | powerfulmedical.com (vendor page) | **Unresolved / COI** | Real paper exists (*Clin Res Cardiol*, DOI 10.1007/s00392-024-02406-5) but the vendor's own pages give inconsistent numbers (1,700 vs 1,770 pts; 95.3% vs other accuracy figures; sensitivity as low as 51.8% on one page). Go to the journal DOI directly, never the vendor page |
| Swedish OMI localization preprint, 540,372 ECGs, C-stat >0.95 | medrxiv 2025.09.11.25335407 | **Confirmed** (abstract-level) | Full text blocked (403); abstract confirms 540,372 ECGs, C-stat >0.95 OMI / >0.87 non-OMI, LM/LAD/LCX/RCA localization. Still a preprint [P] |
| QoH pericarditis/myocarditis specificity 97.7%/87.7%, JACC Advances 2026 | sciencedirect S2772963X26004679 | **Unresolved** | Could not locate this article at all (blocked, not findable by search) — do not cite until found via DOI/PubMed directly |
| Pericarditis DL model, 128 pericarditis ECGs (2022) | PMC9324403 | **Confirmed** | *J Pers Med* 12(7):1150, 2022. 128 pericarditis ECGs (99/10/19 split) vs 66,633 non-pericarditis |

## Digitization (WP2, 01§7)

| Claim | Source | Status | Detail |
|---|---|---|---|
| 2024 Challenge winner = Hough transform + nnU-Net | cinc.org CinC2024-011 / felixkrones/ECG-Digitiser | **Confirmed** | Krones et al. (team SignalSavants), CinC 2024, DOI 10.22489/CinC.2024.097, 1st place, score 12.15. Doc's specific PDF filename not independently confirmed as this paper's ID |
| ECG-Image-Kit | arXiv 2307.01946 | **Confirmed** | Preprint [P], v4 Feb 2024, no journal/conference publication found yet |
| ECG-Image-Database, 35,595 images / 1,977 records | arXiv 2409.16612 | **Confirmed** | Preprint [P]. 1,977 = 977 (PTB-XL) + 1,000 (Emory), arithmetic checks out though not stated verbatim |
| npj Digital Medicine digitization paper, s41746-025-02327-1 | (DOI) | **Unresolved** | Not checked in this pass — verify directly before citing |
| "Printed-PTB" scanned-ECG multi-label study | arXiv 2502.14909 | **Unsupported as described** | Real preprint (Nguyen et al., Feb 2025) on multi-label diagnosis from scanned ECGs, but no mention of "PTB" or a "printed-PTB" dataset found — recheck full text for the actual dataset name |
| Masked training, multi-layout digitized ECGs | arXiv 2508.09165 | **Confirmed** | "PatchECG," Zhang et al., Aug 2025, preprint [P], substantively matches the claim |

## Foundation models / benchmarks (01§7, 02 rhythm)

| Claim | Source | Status | Detail |
|---|---|---|---|
| ICLR 2026 benchmark (26 tasks), ECGFounder among leaders vs transformer SSL | arXiv 2509.25095 | **Corrected** | Real paper, accepted ICLR 2026 ("Benchmarking ECG FMs: A Reality Check"), 8 FMs/26 tasks/12 datasets confirmed — but **ECGFounder is not mentioned on the page at all**; the model confirmed dominant is "ECG-CPC" (a structured state-space model). Likely a model-name conflation in the planning doc — fix before citing |
| FOUND-AF: ECGFounder strongest across AFDB/CinC2017/CPSC2021/LTAFDB | arXiv 2608.03597 | **Unresolved (number), confirmed (paper exists)** | Real Aug-2026 preprint (Taleshinosrati et al.), but the per-dataset "ECGFounder strongest" claim and the specific "30,825 windows / 12.8% prevalence" and "~1.41M LTAFDB segments" numbers could not be found in the abstract/accessible text by two independent passes |
| "How do ECG models scale" | arXiv 2605.17276 | **Confirmed (exists)** | Real May-2026 preprint (Li, Bonassi, et al.), 120+ models, 20K–200M params, CODE dataset ~2.3M records. No specific number was being checked |
| OpenECG | arXiv 2503.00711 | **Confirmed** | 1.2M records, 9 centers, SimCLR/BYOL/MAE comparison |
| PTB-XL v1.0.3: 21,799 ECGs / 18,869 pts / 71 SCP-ECG statements | physionet.org/content/ptb-xl/1.0.3 | **Confirmed exactly** | — |

Note: arXiv IDs in the 2602–2609 range are legitimately real (today is 2026-10-19, so May–Sep 2026 IDs are past-dated, not fake) — don't discard them as impossible on ID alone, but still verify specific numeric claims individually.

## Structural, 5E (02§8, 03§3)

| Claim | Source | Status | Detail |
|---|---|---|---|
| EchoNext (Nature 2025): 1.2M pairs/230k pts; 77%/64% cardiologist comparison; AUCs amyloid 0.95/HCM 0.92/AS 0.90/hypertensive LVH 0.76 | nature.com s41586-025-09227-0 | **Corrected — source conflation** | 1.2M pairs/230k pts and 77%/64% corroborated via secondary press coverage (paywalled primary). **The AUC set (amyloid/HCM/AS/LVH) does NOT belong to EchoNext** — those numbers are from a different paper, Haimovich et al. 2023, *Cardiovasc Digital Health J* (PMC10123506). Fix the attribution in `02_WP5_experts_plan.md` §8 |
| PhysioNet EchoNext dataset: 82,543 pairs, 36,286 pts, 250 Hz | physionet.org/content/echonext | **Corrected** | Official PhysioNet headline is **100,000 ECGs**, 250 Hz; 82,543/36,286 are a downstream paper's train/val/test subset counts, not the dataset's own headline figure |
| Mayo LVEF: AUROC 0.82, sensitivity 26.9% at 0.6% prevalence (Attia 2021) | PMC7955278 | **Confirmed exactly** | AUROC 0.82 (0.75–0.90); sens 26.9% at cutoff 0.256; prevalence 26/4277 = 0.6% |
| Yagi 2022: multi-institution LVEF model generalized worse elsewhere | PMC9779862 | **Confirmed** | Keio-trained model dropped 0.91→0.849–0.877 externally; BWH model held up better (0.905–0.915) |
| LVH etiology models paper | PMC10123506 | **Confirmed — and is the true source of the AUCs above** | Haimovich et al., *Cardiovasc Digital Health J* 2023; amyloid 0.95–0.96, HCM 0.92, AS-LVH 0.90, hypertensive LVH 0.76 |
| HCM external validation Bern/Oxford/Seoul | boris.unibe.ch/199415 (Siontis et al., *EHJ-DH* 2024) | **Confirmed** | 773 HCM vs 3,867 controls, overall AUC 0.922 |

## Electrolyte / QT / channelopathy, 5F (02§8)

| Claim | Source | Status | Detail |
|---|---|---|---|
| Mayo hyperkalemia, JAMA Cardiol 2019, AUC 0.883/0.860/0.853, ~90% sens, spec 63/55/55% | Galloway et al., *JAMA Cardiol* 2019;4(5):428-436 | **Confirmed** | AUC 0.883/0.860/0.853; sens ~89–91%; spec 63.2/54.7/55.0% — matches closely |
| Single-lead hyperkalemia preprint, AUROC 0.861 external K⁺≥6.0 | arXiv 2603.14177 | **Confirmed** | "Pocket-K," Mar 2026 preprint, external AUROC 0.861 for K⁺≥6.0 exactly as claimed |
| LQTS Amsterdam/Leuven AUC 0.89–0.92 | PMC9063181 | **Confirmed** | Aufiero et al., *BMC Medicine* 2022; Amsterdam internal CV 0.89–0.92, Leuven external 0.89–0.90 |
| HiRO CNN beat expert QTc incl. concealed LQTS, JAMA Cardiol 2024 | jamanetwork 2815659 | **Confirmed** | Sensitivity 0.90 (CNN) vs 0.36 (expert QTc); concealed-LQTS sensitivity 0.78 for CNN |
| Brugada PNAS-family 2023, AUC 0.934; public "Brugada-HUCA" dataset | PMC10627411 (Melo et al., *PNAS Nexus* 2023) | **Corrected** | Journal and AUC (0.934±0.027, n=370 external) confirmed. **"Brugada-HUCA" dataset name is not supported** — the real public data is an unnamed Dryad repository |
| WPW preprint, pediatric 12-lead DL | medrxiv 2024.09.26.24314432 | **Corrected — mischaracterized** | Real preprint exists, but it is "Detecting WPW from Lead-I ECG" — **single-lead (Lead I), adult PTB-XL data**, not pediatric, not 12-lead. Fix description in `02_WP5_experts_plan.md` §8 |

## Delineation (5G) and uncertainty (02§8)

| Claim | Source | Status | Detail |
|---|---|---|---|
| LUDB: 200×10s, 12-lead, 500 Hz, ODC-By | physionet.org/content/ludb | **Confirmed exactly** | — |
| DL segmentation with external QTDB/LUDB tests | arXiv 2304.06237 | **Confirmed** | Joung et al.; peer-reviewed version also exists (PMC11175442); F1 >97–99% on LUDB |
| Vranken 2021 (UMC Utrecht): ensembles most robust, 526,656 ECGs, six methods | *Eur Heart J Digit Health* 2021 | **Corrected** | 526,656 ECGs and six methods confirmed, but the best performers were **hybrid** methods (variational-inference+Bayesian-decomposition, ensemble+auxiliary-output), not plain ensembles alone — "ensembles most robust" oversimplifies the paper's own conclusion. (The source paper also has a minor internal arithmetic inconsistency: 526,656 vs. a breakdown summing to 518,744 — not our error, theirs.) |
| Emory conformal risk control preprint, multi-label ECG | cs.emory.edu/~jyang71/files/ea-crc.pdf | **Confirmed** | "Evidence-Augmented Conformal Risk Control..." — PTB-XL, 23 classes, micro-F1 0.568→0.574 at α=0.10 |

## Rhythm datasets (02§8)

| Claim | Source | Status | Detail |
|---|---|---|---|
| Leipzig Heart Center: SVT, VT, pre-excitation, pacing | physionet.org/content/leipzig-heart-center-ecg | **Partially confirmed** | SVT/VT/pre-excitation confirmed; "pacing" not clearly confirmed — dataset documentation mentions ablation-catheter electrograms, not clearly pacing |
| VitalDB arrhythmia | physionet.org/content/vitaldb-arrhythmia | **Confirmed exists** | 482 patients, intraoperative, 10 rhythm categories. No specific number was being checked |

## Novelty / mixture-of-experts (03§3) — feeds the novelty memo

| Claim | Source | Status | Detail |
|---|---|---|---|
| Multi-expert ensemble, 254 ECG labels | PMC12912303 = *npj Cardiovasc Health* 2024;1:8 | **Confirmed** | 12 expert sub-models with mutually-exclusive/symbiotic label-correlation loss; AUROC 0.973 offline/0.956 online. Lists fiducial+AI fusion as **future work**, does not test it — no H1 overlap |
| "MoE transformer, ITU thesis" | Atalay Çelik M.Sc. thesis, ITU, Jul 2025 | **Confirmed, located** | 26-label MoE-transformer classifier, 81,926 records, 6 datasets. No fiducial-disagreement or safety-routing content |
| ExChanGeAI sparse-MoE encoder, learned router | arXiv 2503.13570 | **Confirmed** | Trainable router (top-k noisy/Gumbel-Softmax), "Mixture of Architectures" feeding the CardX FM. No H1/H3 overlap |
| EnECG, Emory preprint | arXiv 2511.22935 | **Confirmed, located** | LoRA-tuned heads on several non-ECG foundation models + learned MoE weighting for compute efficiency. No fiducial-disagreement or safety-routing content |

## LLM / reasoning / explainability (01§7) — background, lower priority

| Claim | Source | Status | Detail |
|---|---|---|---|
| arXiv 2607.20814 | arxiv.org/abs/2607.20814 | **Confirmed exists** | "Enhancing Explainable Cardiac Diagnosis with Guide-Grounded Multimodal LLMs," Jul 2026 |
| NeurIPS 2025 poster 132675 | neurips.cc/virtual/2025/poster/132675 | **Confirmed** | "Exploring RAG-driven Multimodal LLMs for Explainable ECG Interpretation," FM4BB workshop |
| CARE-ECG | arXiv 2604.10420 | **Confirmed** | Apr 2026 preprint, matches title |
| ECG-Chat | arXiv 2408.08849 | **Confirmed** | matches exactly |
| Q-Heart | arXiv 2505.06296 | **Confirmed** | matches exactly |
| Review paper | arXiv 2604.02501 | **Confirmed** | "ECG FMs and Medical LLMs for Agentic CV Intelligence at the Edge," self-described survey |
| "Saliency pitfalls" paper | arXiv 2609.25978 | **Corrected — wrong domain** | ID resolves and is genuinely about saliency-map faithfulness pitfalls, but it's a **breast MRI** case study, not ECG. Fix description |
| arXiv 2508.17294 | arxiv.org/abs/2508.17294 | **Confirmed** | "XAI for Arrhythmia detection," SHAP/saliency comparison |

## Regulatory and reporting guidelines (01§1, 03§4.8)

| Claim | Source | Status | Detail |
|---|---|---|---|
| CDSCO/MD/GD/MDSW/01/2026, 21 Jul 2026, SaMD classification | secondary legal coverage (Nishith Desai, NatLawReview, etc.) — primary cdsco.gov.in PDF not directly fetched | **Corrected** | Document number/date confirmed via secondary sources. **The final guidance drops the "SaMD/SiMD" terminology** used in the draft, replacing it with a function-based "medical purpose" test — diagnosis/decision-support software is "likely to be treated as a medical device" under that test, not under an "SaMD" label. Fix the framing in `01_project_context.md` §1. Primary PDF still needs a direct read [V] |
| TRIPOD+AI, BMJ 2024;385:e078378 | Collins et al., *BMJ* 2024 | **Confirmed** | DOI 10.1136/bmj-2023-078378 |
| STARD-AI, Nature Medicine 2025;31:3283-3289 | Sounderajah et al., *Nat Med* 2025 | **Confirmed** | Page range confirmed correct — the doc's own [V] flag can be cleared |

## Summary of mismatches vs the docs (needs owner/team correction before drafting)

1. **5A**: "QoH" Portuguese cohort number actually belongs to "PMcardio-OMI" — confirm whether these are the same product or a mislabel.
2. **Foundation models**: the ICLR 2026 benchmark does not mention ECGFounder at all; the leading model it names is "ECG-CPC." Likely conflation — correct before citing in WP4's backbone bake-off rationale.
3. **5E**: the amyloid/HCM/AS/hypertensive-LVH AUC set is misattributed to EchoNext; it belongs to Haimovich et al. 2023. EchoNext's own headline dataset size is 100,000 ECGs, not 82,543 (that's a downstream split count).
4. **5F**: the WPW "pediatric 12-lead" preprint is actually adult, single-lead (Lead I), PTB-XL-based.
5. **5F**: "Brugada-HUCA" is not a real dataset name — the public data is an unnamed Dryad repository.
6. **Uncertainty**: Vranken 2021's finding is better stated as "hybrid methods (VI+Bayesian-decomposition, ensemble+auxiliary-output) were most robust," not "ensembles were most robust."
7. **Regulatory**: the CDSCO guidance does not use "SaMD" terminology in its final form; it uses a function-based "medical purpose" test. Update `01_project_context.md` §1's framing.
8. **Vendor COI**: the QoH German CPU cohort numbers are inconsistent across Powerful Medical's own pages — use the journal DOI (10.1007/s00392-024-02406-5), not the vendor page, for any number quoted in a draft.
9. Two sources could not be located/verified at all and should not be cited until found directly: the JACC Advances pericarditis/myocarditis specificity claim, and the npj Digital Medicine digitization DOI (s41746-025-02327-1, not checked in this pass).
10. The "printed-PTB" scanned-ECG study's dataset name was not found in its abstract as described — recheck full text.

## What this pass did NOT do (limits)
- Several sources returned 403/503 on direct fetch (PMC, medRxiv) and were corroborated via independent secondary coverage/search instead of a full primary-text read — treat those as **weaker** confirmations than a direct read, and re-check directly before anything load-bearing goes in a submitted manuscript.
- The npj Digital Medicine digitization DOI (s41746-025-02327-1) was not checked.
- The novelty search for H1 (see `novelty_memo.md`) was not exhaustive — one more targeted pass (e.g. "automated ECG measurement discordance," "vendor-algorithm-vs-model disagreement") was flagged by the agent as worth running before calling H1's novelty settled.
- PhysioNet's stated policy on using credentialed data with hosted/online LLM services (flagged [V] in `01_project_context.md`) was not looked up in this pass — still open.
