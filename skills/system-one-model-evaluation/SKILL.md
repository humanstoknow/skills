---
name: system-one-model-evaluation
description: Compare Jev, open System One decision models, and a current rules or LLM baseline for an already defined typed decision. Use when choosing a model or planning a fair trial; not for finding the decision itself.
---

# System One Model Evaluation

Help the user decide whether a typed decision should run through hosted Jev, a suitable open model, or the workflow they already have. Produce a comparison that could change the decision, including a clear case for keeping the baseline. Do not assume API compatibility means equal judgments or trustworthy probabilities.

## Start with one decision contract

Use the user's existing contract if they have one. Otherwise ask for the actual state sent to the decision step, the question and allowed answers, the downstream action, the cost of each consequential mistake, expected volume, and privacy or deployment limits. If some fields are unavailable, give a provisional plan that names them as pending rather than inventing them. A discovery skill such as Jev Workflow Design can help find a decision, but this skill starts once that decision is bounded. If the job is a calculation, exact lookup, permission check, or open-ended generation, recommend the appropriate rules, database, or LLM path and stop.

Keep the current rules or LLM as a candidate. Shortlist only models that can represent the needed question type, fit the data and deployment constraints, and be tested on the same task. Check current primary documentation before stating model capabilities, license, limits, API shape, or pricing. Separate a model and its weights from a server that only imitates another provider's API. Record each candidate's exact model/checkpoint revision, runtime and adapter version, serving settings, and hardware; mark these as required manifest fields when planning an unrun trial. A changed checkpoint or prompt is a new candidate.

## Make the comparison fair

Build a representative set of cases labeled by a qualified person, with the expected answer, consequence of error, and independent labels for high-risk subsets such as billing disputes. Include ordinary examples and the cases that matter to this workflow: ambiguous or missing evidence, negation, long inputs, many labels, out-of-scope inputs, and relevant languages or formats. Size the held-out set around the rare, costly errors the user must detect; report uncertainty and do not claim safety from a small sample with zero observed failures. Keep training, threshold/calibration, and final test cases separate. Do not use a model's agreement with another model as ground truth.

Where possible, use the same state and question bytes for every candidate. Document any translation needed for a model's schema and test that translation separately. Check truncation and option handling rather than assuming a successful HTTP response means the whole question was evaluated. For models that produce different probability or confidence semantics, fit thresholds independently on the calibration split. Never copy a Jev threshold to Laya or treat a reported confidence as the chance that the whole workflow will succeed.

Run the paired trial if the user requested execution and access, data transfer, and compute are authorized. Otherwise specify the smallest runnable trial and label every result as unmeasured. Never invent benchmark, cost, or accuracy figures. Published vendor or community benchmarks can inform the shortlist, but they do not replace the user's held-out cases.

## Measure the decision and the workflow

Report per-candidate:

- **Judgment:** correct decisions, consequential false positives and negatives, abstentions or escalations, and performance by meaningful slice.
- **Probability quality:** calibration and threshold behavior where a probability is actually returned. Distinguish a choice score, a binary yes probability, and workflow-level correctness.
- **Operations:** p50/p95 latency, cold start or model-load time, throughput at expected concurrency, failures, retries, and full end-to-end latency.
- **Total cost and constraints:** hosted charges or compute plus memory, serving, maintenance, data residency, license, and availability. State the workload and hardware behind any estimate.

Compare the same downstream action, not only isolated model output. A faster classifier adds little if a later step dominates latency; a wrong route may cost more than the model call. Use the user's error costs and capacity constraints to decide which tradeoffs are acceptable.

## Recommend and hand off

Return a compact compatibility matrix and test manifest. Use this results skeleton, with one row per candidate; mark unavailable values as pending rather than filling them from another workload:

| Candidate and exact version | Evidence status | Labeled cases (high-risk cases) | Consequential errors by type | Threshold and calibration | End-to-end p95 | Total cost at expected volume |
| --- | --- | --- | --- | --- | --- | --- |
| Current baseline | Measured / unrun | n (n) | Count and consequence | Rule or calibrated threshold | Measured or pending | Measured or estimated, with assumptions |
| Candidate | Measured / unrun / vendor-claimed | n (n) | Count and consequence, or pending | Candidate-specific threshold and calibration result, or pending | Measured or pending | Measured or estimated, with assumptions |

Label each figure as measured on the user's cases, unrun, or vendor-claimed; vendor figures are context, not trial results. State the unknowns and sample-size limits beneath the table. Then recommend one of: adopt a named pinned candidate, run a narrower trial, or keep the baseline. Adoption requires measured evidence on the user's decision and a stated acceptance threshold; otherwise return a trial plan or keep the baseline. Explain which evidence would reverse the recommendation. If adoption is supported, propose a shadow period with the existing system in control, explicit review criteria, and a rollback to that baseline. Do not switch live routing or transmit private examples without the user's authorization for those actions.

Current documentation to verify before an implementation: [TypeSafe System One](https://docs.typesafe.ai/concepts/system-one), [TypeSafe models](https://docs.typesafe.ai/models), and [Laya's source and model documentation](https://github.com/NandhaKishorM/laya). These links are starting points, not a frozen catalog of candidates.
