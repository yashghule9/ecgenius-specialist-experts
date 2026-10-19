# Open questions for the owner

Decisions from `docs/04_claude_code_backlog.md` §4, plus related items surfaced while reading the plan. Nothing past T1/T15 proceeds on these until answered.

## Assets to hand over (blocking T2 and T7)
1. **LightECGNet v2** — where does the code live, what's the interface (input format, sampling rate, output heads, checkpoint), and what's its licence? Needed as the baseline for every expert.
2. **35-node ontology** — the mapping file (SCP-ECG/SNOMED → ontology nodes) is referenced throughout but not in these docs. Please share it; T2 will not recreate it from scratch.
3. **Digitizer pipeline** (U-Net/perspective/TPS) — code location and interface, for the digitization arm (T13) and WP2 handoff.
4. **Bayesian reasoning engine / SHAP-ablation panel / RBAC** — not needed for WP5 Paper 1 directly, but flag location now if available.

## Paper 1 scope
5. Confirm Paper 1 experts: default is 5G + 5B + 5C + 5E (`03_paper_strategy.md` §1, §4.2). Accept, or fall back to 5G+5B+5C+5F-QT if EchoNext credentialing is slow?
6. Target venue for Paper 1 — npj Digital Medicine is the stated first choice; confirm or pick from the shortlist in `03_paper_strategy.md` §7 (none of these have been checked for scope/APC/preprint policy yet — that's part of T15).

## OMI (5A) strategy
7. Which Indian hospital(s), if any, can provide cath-lab-linked ECGs (culprit-occlusion label, troponin trajectory, door-to-cath times), and in what form (paper/photo/raw)? This is the single biggest gate on Paper 2.
8. If no partner data by week 8: do you accept shipping 5A as research-only on the MIMIC proxy (Tier C, PCI-procedure-code based)? Confirm you understand that label is not a true occlusion label.

## Clinical input
9. Who are the cardiologist/EP advisors, and what's their time budget for (a) the 300-ECG adjudication subsets (5C conduction, mimic panels) and (b) choosing safety-tier sensitivity targets (OMI, hyperkalemia, high-grade AV block)?
10. Is hyperkalemia allowed to alert at low specificity (Mayo-model range: ~90% sens / 55–63% spec), or should it ship as an advisory-only output given the PPV implications at low prevalence?

## Safety tier scope
11. For Paper 1, which safety-tier experts are real (trained, evaluated) vs simulated/stubbed? The plan's default safety tier (OMI/STEMI, VT/VF, high-grade AV block, severe hyperkalemia, long QT) is broader than what Paper 1's expert set (5G/5B/5C/5E) can actually deliver — state this explicitly in the paper or scope it down.

## Infrastructure
12. Compute available (GPU type/count, shared vs dedicated) for the backbone bake-off (WP4) and expert training.
13. Where is credentialed data (MIMIC-*, EchoNext, HEEDB) allowed to be processed — local workstation, institutional server, specific cloud account? Confirm this before any pipeline reads credentialed data, per the "credentialed data stays local" rule.
14. Team roster (the plan says ~4–6 people) — who owns which work package, so task assignments in the backlog can be matched to people.
