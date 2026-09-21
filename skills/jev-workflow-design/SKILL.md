---
name: jev-workflow-design
description: Find where TypeSafe AI's Jev can add value in an existing agent workflow, define the decision and fallback, and design a comparison with rules or the current LLM. Use when evaluating or integrating Jev.
---

# Jev Workflow Design

**A System One model turns supplied context into a bounded decision and probabilities that software can act on.**

Help the user find one worthwhile Jev decision in their own workflow—or explain why they should keep their current approach. Deliver a concrete design, not a catalogue of AI ideas. No API access is needed for this design stage.

## Understand the division of work

LLMs can already classify and return schema-constrained JSON. Jev's proposed advantage is specialization: TypeSafe describes training for calibrated decisions and a parallel sampler for decision probabilities, rather than token-by-token text generation. Independent questions can share one request. It returns Choice, Score or Noul answers, without writing prose or reasoning explanations. Measure the advantage on this workload; valid output types do not guarantee correct judgments.

Keep calculations, permissions, exact lookups and execution in code. Keep writing, exploration and multi-step reasoning with the agent's LLM. Use Jev for a focused judgment whose evidence is available now. It currently accepts text and structured text; another tool must prepare images or audio.

## Find the decision worth separating

Inspect the user's task, existing workflow and available examples. If these are missing, ask what arrives, what happens next, and which repeated decision causes delay, expense or mistakes. Do not require a technical architecture description.

Consider at most three candidates. For each identify the decision, frequency, present decision-maker, consequence of an error and downstream action. Prefer a repeated semantic judgment with known alternatives and a cheap way to defer uncertainty. Possible candidates include choosing a specialist, selecting useful context or checking a proposed action against a request.

Reject candidates that ordinary code can settle, need unavailable evidence, or require Jev to generate or reason through the whole solution. A one-off task may be cheaper to finish with the current agent. Unknown volume or performance makes a recommendation provisional; do not invent savings. If no candidate fits, give the simpler alternative and stop; do not manufacture a Jev trial.

## Specify one decision contract

For the best candidate, provide:

- **Placement:** the exact point before or after an existing step where the judgment is needed.
- **State:** the smallest sufficient evidence, its provenance and freshness, including relevant exceptions. Missing evidence goes to a fallback; never silently truncate it away.
- **Question and rubric:** explicit boundary cases and permitted outputs. Choice selects one exclusive alternative; add a meaningful no-match/insufficient-evidence option. Use separate Noul questions for overlapping labels. Score describes an ordered dimension, not a count or calculation.
- **Routing:** what code does with each answer, uncertainty and API failure. Different consequences may need different thresholds. Choose thresholds from labelled examples and error costs, not a universal 0.8 default.
- **Division of responsibility:** what the LLM still writes or reasons about, and what code still enforces.

Batch only independent questions over shared evidence. Questions cannot read sibling answers; dependent decisions need another step. Record judgments against the state they evaluated; recheck if the proposed action or evidence changes.

Choice/Score confidence describes probability concentration, not the probability that the whole workflow is correct. Noul returns P(yes) in [0,1], without separate confidence: near zero means likely **no**, near one means likely **yes**, and near 0.5 is uncertain. No prediction grants permission or overrides a hard rule.

## Prove value before changing the workflow

Propose a shadow trial: Jev logs a recommendation while the existing system remains in control. Compare against current rules or the current LLM on the same representative inputs. Include clear cases, near misses, missing information, conflicting intent and hostile instructions embedded in input. Have a person label outcomes; another model's opinion is not ground truth.

Define success for the user’s actual task, then measure consequential errors, unnecessary escalations, completed-task success, end-to-end latency and total cost including retries and review. Keep tuning and evaluation examples separate. Recommend adoption only if the agreed quality and cost/latency goals hold; otherwise retain the baseline. Label unrun tests as unrun and never invent confidence or performance numbers.

## Handoff

Return the recommended decision (or no-fit conclusion), why it beats the alternatives, the contract, and the smallest useful comparison trial. Use language appropriate to the user's role. Only implement or call Jev when that work is requested and data transfer is authorized; reuse existing authorization.

For a worked action-scope contract and the current API handoff, read [references/decision-example.md](references/decision-example.md). Before integration, verify the [API](https://docs.typesafe.ai/api), [model limits](https://docs.typesafe.ai/models), [confidence semantics](https://docs.typesafe.ai/confidence) and [known limitations](https://docs.typesafe.ai/model-jaggedness/jev-1.13). Use a securely configured TYPESAFE_API_KEY; never put secrets into chat or artifacts. Pin the evaluated model version before relying on tuned thresholds.
