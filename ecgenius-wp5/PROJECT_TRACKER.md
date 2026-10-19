# Project tracker

Shared status for everyone (and every Claude session) working on this repo. Update the row **before** you commit the work it describes — status/owner/date, one line in Notes on what changed or what's blocking. Task IDs and acceptance criteria are defined in `docs/04_claude_code_backlog.md` §2.

Status values: `not started` · `in progress` · `blocked` · `done`.

| ID | Task | Status | Owner | Last updated | Notes |
|---|---|---|---|---|---|
| T0 | Scaffold repo + licence audit | **done** | Claude (session 1) | 2026-10-19 | `docs/licences.md`. Flags: ST-MEM, HuBERT-ECG, "ECG-CPC" licences unresolved — resolve before WP4 locks a backbone |
| T1 | Data-access tracker | **done** | Claude (session 1) | 2026-10-19 | `DATA_ACCESS_TRACKER.md`. MIMIC-IV-ECG confirmed Open Access (not credentialed); MIMIC-IV, MIMIC-IV-ECHO, EchoNext, HEEDB still need credentialing — none requested yet |
| T2 | Ontology and label map v1 | **blocked** | — | — | Waiting on owner to hand over the existing 35-node ontology file (`OPEN_QUESTIONS.md` #2) — do not recreate it |
| T3 | Dataset loaders and manifests | not started | — | — | Depends on T2 (label map) and T4 (splits) conventions being settled first |
| T4 | Frozen splits | not started | — | — | |
| T5 | Eval harness | not started | — | — | |
| T6 | 5G measurement layer | not started | — | — | |
| T7 | Baselines (incl. LightECGNet v2) | **blocked** | — | — | Waiting on owner for LightECGNet v2 code/interface (`OPEN_QUESTIONS.md` #1) |
| T8 | 5B rhythm expert | not started | — | — | |
| T9 | 5C conduction expert | not started | — | — | |
| T10 | 5E structural expert | **blocked** | — | — | Needs EchoNext credentialing (see `DATA_ACCESS_TRACKER.md`) |
| T11 | Uncertainty stack | not started | — | — | |
| T12 | Safety tier + router + arbitration | not started | — | — | This is WP6, not WP5 — kept out of this repo's folder layout for now (see commit removing `routing/`) |
| T13 | Digitization arm | not started | — | — | Needs owner's existing digitizer code (`OPEN_QUESTIONS.md` #3); `digitization/` folder removed from scaffold until this starts |
| T14 | Paper 1 assembly | not started | — | — | |
| T15 | Literature verification + novelty search | **done** | Claude (session 1) | 2026-10-19 | `docs/verification_log.md`, `docs/novelty_memo.md`. Found several claim mismatches in `docs/02_WP5_experts_plan.md` — see verification log's "Summary of mismatches" section before citing anything from it |
| T16 | OMI expert (gated) | not started | — | — | Starts only when partner cath-lab data arrive |

## Other open items (not backlog task IDs)
| Item | Status | Notes |
|---|---|---|
| `OPEN_QUESTIONS.md` | **open — 14 questions** | Owner decisions needed before T2/T7/T13 can start; nothing past T1/T15 should proceed until these are answered |
| Mismatches found in T15 | **open** | 10 corrections needed in the planning docs themselves (wrong attributions, mischaracterized sources) — listed at the bottom of `docs/verification_log.md` |

## Convention for the next person (human or Claude)
1. Read this file and `OPEN_QUESTIONS.md` first — don't re-derive status from git history.
2. Before starting a task, change its row to `in progress` and put your name/session in Owner.
3. When done, change to `done`, fill Last updated, and link the file(s) it produced in Notes.
4. If you get stuck on an owner decision, set status to `blocked` and point to the relevant `OPEN_QUESTIONS.md` item instead of guessing.
