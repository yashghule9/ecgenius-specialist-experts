# Novelty memo — Paper 1 hypotheses H1–H3

Based on the T15 verification pass (`verification_log.md`). This is a status check, not a guarantee — see limits at the end.

## H1: measured-vs-neural disagreement predicts neural errors, beyond ensemble variance
**Status: appears novel, not found already done — but the search was not exhaustive.**

Four mixture-of-experts/ensemble papers were checked directly (multi-expert ensemble for 254 labels, npj Cardiovasc Health 2024; an ITU M.Sc. thesis on MoE-transformer multi-label ECG; ExChanGeAI sparse-MoE; EnECG). None tests disagreement between rule-based fiducial measurements (PR/QRS/QT intervals) and a neural network's output as an error signal. The closest adjacent result: the 254-label ensemble paper computes traditional fiducial-point measurements *alongside* its AI labels but explicitly lists fusing the two as **future work** — it doesn't test it.

A broader search surfaced a different, more mature adjacent field: ensemble-variance / Bayesian uncertainty methods, where high model uncertainty is shown to correlate with *cardiologist label disagreement*. That's related but not the same claim — none of those papers bring in rule-based measured values as the comparison signal. So H1 as specifically framed looks like a real gap, not a demonstrated result elsewhere.

**Action before treating this as settled:** run one more targeted search ("automated ECG measurement discordance," "vendor-algorithm-vs-model disagreement," "Marquette/Glasgow algorithm discordance") — this was flagged by the verification pass as the next thing to check, not yet done.

## H2: conformal sets keep nominal coverage across datasets and under digitization shift
**Status: the general conformal-for-ECG line is active and real, but the specific digitization-shift-coverage test was not found anywhere in this pass.**

An Emory preprint on evidence-augmented conformal risk control for multi-label ECG was confirmed (real, PTB-XL, 23 classes). No source checked tests conformal coverage specifically *under a digitization/image-reconstruction shift* — that combination (conformal + digitization shift) was not found in the literature surveyed. This is consistent with it being a real gap, but this pass searched conformal-ECG sources named in the docs, not a fresh search for "conformal prediction digitization shift" as its own query — do that before finalizing the novelty claim.

## H3: safety-tier-always-on + recall-tuned router matches run-all-and-fuse sensitivity at lower cost
**Status: partially anticipated outside ECG; the exact combination (non-bypassable tier + recall router + explicit run-all-fuse compute/latency comparison) was not found anywhere, in or outside ECG.**

Four non-ECG medical-AI routing papers were found: PRISM-Consult (selective expert routing, ~1.57 experts/episode, recall_any=1.0 — the efficiency half of H3, no run-all-fuse comparison found); a post-discharge multi-agent framework with a hard-coded, model-independent escalation tier for life-threatening cases that explicitly bypasses the generative pipeline (conceptually close to "safety tier that cannot be routed away," but again no run-all-fuse benchmark found); Tiered Agentic Oversight; MPD²-Router. None combines all three elements the paper needs: (a) a tier that cannot be routed away, (b) a router explicitly tuned for recall, (c) a stated comparison against running every expert and fusing. **This combination, in or outside ECG, was not found** — reasonable grounds for the novelty claim, but note all four sources are preprints, not peer-reviewed.

## Bottom line for drafting
- Don't claim "MoE/ensembles for ECG are novel" — they aren't (per `03_paper_strategy.md` §3, already correctly hedged).
- H1 and H3 look like real, defensible gaps on what's been checked so far; H2's specific digitization-shift angle wasn't directly searched yet.
- Position the paper's novelty on the *combination and evaluation framework* (safety tier that can't be bypassed + recall-tuned router ablated against run-all-and-fuse + measured-vs-neural disagreement as an incremental error signal), not on any single piece being unprecedented — this matches the reviewer-risk mitigation already planned in `03_paper_strategy.md` §4.9.
- Before submission, run the follow-up searches flagged above for H1 and H2; treat this memo as a first pass, not a cleared novelty search.

## Limits of this novelty search
Search was English-language, web-search-driven, not a systematic database review (no PubMed/IEEE/ACM structured query, no citation-chasing from the four MoE papers found). A real pre-submission novelty check should also search Google Scholar/PubMed directly with the exact H1–H3 phrasing and chase citations of the four MoE/ensemble papers confirmed here.
