# Data access tracker

Credentialed/restricted datasets only. Never commit raw records or row-level derived data from any dataset marked "credentialed" or "restricted" below. Update status/date as access moves.

| Dataset | Access type | Status | Owner | Requested | Granted | Notes |
|---|---|---|---|---|---|---|
| MIMIC-IV-ECG v1.0 | **Open Access (ODbL v1.0)** — no credentialing needed (verified 2026-10-19; was credentialed in earlier versions) | **no credential needed — just download** | TBD | — | — | needed for 5A Track 2 (proxy OMI), 5C external (Tier C), 5F (K⁺/QTc). Still gated by `01_project_context.md`'s "credentialed data stays local" rule only if linked to MIMIC-IV hosp/ED (which IS credentialed) |
| MIMIC-IV | PhysioNet credentialed | not started | TBD | — | — | hosp/ED/labevents linkage for 5A, 5F; this is what actually requires the DUA, not MIMIC-IV-ECG itself |
| MIMIC-IV-ECHO | PhysioNet credentialed | not started | TBD | — | — | 5E external; linkage to MIMIC-IV-ECG unconfirmed [V] |
| EchoNext | PhysioNet restricted (DUA, per-dataset request) | not started | TBD | — | — | 5E primary training/eval data; Paper 1 gate per `03_paper_strategy.md` §4.2 |
| HEEDB | BDSP credentialed | not started | TBD | — | — | not required for Paper 1 scope (5G+5B+5C+5E); track for later papers |
| Indian hospital partner (cath-lab-linked ECGs) | institutional/IRB, not a public dataset | not started | TBD (owner to identify) | — | — | gates 5A Track 1; open question, see `OPEN_QUESTIONS.md` |

## Public, no credentialing needed (for reference, not tracked above)
PTB-XL, CODE-15%, Chapman-Shaoxing, AFDB, CinC2017, CPSC2021, LTAFDB, LUDB, QTDB, Leipzig Heart Center ECG, VitalDB arrhythmia — open PhysioNet/public access; confirm individual licence terms in `docs/licences.md` before redistribution or commercial use. MIMIC-IV-ECG (the waveform dataset alone, not linked to hosp/ED) also belongs in this category as of the current PhysioNet release — see `docs/licences.md`.

## Process notes
- PhysioNet credentialing requires a CITI "Data or Specimens Only Research" course completion and a signed data use agreement per project/dataset.
- Record the exact DUA version and signature date once granted (DUAs can change between versions).
- PhysioNet's policy on using credentialed data with hosted/online LLM services is marked [V] in `01_project_context.md` — read and log it here before any credentialed data touches any API-based tool.
