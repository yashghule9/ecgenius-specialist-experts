# Licence audit (T0)

Checked against primary sources (PhysioNet dataset pages, GitHub repos/API, Zenodo) on 2026-10-19. Items marked [V] need a manual follow-up before relying on them for anything commercial or redistributable. LightECGNet v2 is the owner's private model — not audited here; request licence terms directly.

## Datasets

| Dataset | Licence | Commercial use | Redistribution (data) | Status |
|---|---|---|---|---|
| PTB-XL v1.0.3 | **CC BY 4.0** (corrected — not ODC-BY; older versions were ODC-BY, 1.0.3 changed) | Yes | Yes, with attribution | Confirmed |
| MIMIC-IV-ECG v1.0 | **ODbL v1.0**, now **Open Access** (corrected — no longer credentialed) | Yes | Yes, but derivative databases must stay ODbL (share-alike) | Confirmed |
| MIMIC-IV-ECHO v1.0.1 | PhysioNet Credentialed Health Data License 1.5.0 | No (research-only) | No | Confirmed |
| CODE-15% | CC BY 4.0 | Yes | Yes, with attribution | Confirmed |
| Chapman-Shaoxing | CC BY 4.0 | Yes | Yes, with attribution | Confirmed |
| HEEDB | BDSP Restricted Health Data License 1.0.0 (controlled, via BDSP) | No | No | Confirmed as restricted; exact DUA clauses need a legal read [V] |
| EchoNext | PhysioNet Restricted Health Data License 1.5.0 | No | No | Confirmed |
| AFDB (MIT-BIH AF) | ODC-BY v1.0 | Yes | Yes, with attribution | Confirmed |
| CinC2017 | ODC-BY v1.0 | Yes | Yes, with attribution | Confirmed |
| CPSC2021 | CC BY 4.0 | Yes | Yes, with attribution | Confirmed |
| LTAFDB | ODC-BY v1.0 | Yes | Yes, with attribution | Confirmed |
| LUDB v1.0.1 | ODC-BY v1.0 | Yes | Yes, with attribution | Confirmed |
| QTDB | ODC-BY v1.0 | Yes | Yes, with attribution | Confirmed |
| Leipzig Heart Center ECG v1.0.0 | ODC-BY v1.0 | Yes | Yes, with attribution | Confirmed |
| VitalDB-arrhythmia v1.0.0 | CC BY 4.0 | Yes | Yes, with attribution | Confirmed |

## Candidate backbone models

| Model | Licence | Commercial use | Redistribution (weights) | Status |
|---|---|---|---|---|
| ECGFounder | MIT | Yes | Yes | Confirmed (github.com/PKUDigitalHealth/ECGFounder) |
| ECG-FM | MIT | Yes | Yes | Confirmed (github.com/bowang-lab/ECG-FM) |
| ECG-JEPA | MIT | Yes | Yes | Confirmed, **but two different repos share this name** (sehunfromdaegu/ECG_JEPA and kweimann/ECG-JEPA, both MIT) — confirm which one before use [V] |
| ST-MEM | No LICENSE file (GitHub reports "Other/NOASSERTION") | Unknown | Unknown | **[V] unresolved** — default is all-rights-reserved until authors clarify |
| HuBERT-ECG | No LICENSE file in repo; third-party Hugging Face checkpoints labelled CC BY-NC 4.0 (not the repo's own declaration) | No, if the HF label holds | Conditional/No | **[V] unresolved** — confirm with authors directly |
| MERL | MIT | Yes | Yes | Confirmed (github.com/cheliu-computation/MERL-ICML2024) |
| ECG-CPC | No canonical repo found under this exact name. Closest candidate (helme/ecg_ptbxl_benchmarking, which includes a CPC variant) is **GPL-3.0** — copyleft | Unknown/conditional | Conditional (GPL-3.0) | **[V] unresolved** — name the exact intended implementation before any licence claim |
| S4/ResNet baseline | standard architectures, no single canonical licensed repo | — | — | not applicable as a "model" licence — depends on implementation chosen |
| LightECGNet v2 | owner's private model | N/A | N/A | not audited — request licence terms from owner |

## Key flags
- **MIMIC-IV-ECG is now Open Access (ODbL), while MIMIC-IV-ECHO stays Credentialed.** `DATA_ACCESS_TRACKER.md` updated accordingly — do not conflate the two when planning access steps.
- **EchoNext and HEEDB are both restricted/credentialed, non-commercial, no-redistribution.** Any backbone pretrained on either cannot have its weights freely redistributed without legal review — relevant to WP4's "check licences" gate and to the Paper 1 code-availability statement.
- Three backbone candidates (ST-MEM, HuBERT-ECG, "ECG-CPC") have unresolved or copyleft licensing — resolve before WP4's bake-off locks in a default backbone, since this affects whether Paper 1 can release code/weights.
