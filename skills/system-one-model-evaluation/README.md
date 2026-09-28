# System One Model Evaluation

Compare hosted Jev, suitable open decision models, and your current rules or LLM on one already defined decision. The skill asks for a labeled workload and consequential error costs, then produces a paired trial plan or a measured recommendation. If the evidence is insufficient or the baseline wins, it should say so.

## Use

Load [SKILL.md](SKILL.md) through your agent’s supported skill mechanism. Give the agent the decision’s input state, question, allowed answers, downstream action, current implementation, and constraints.

Ask: “Compare Jev, viable open models, and my current approach on this decision. Show the evidence we need, keep vendor claims separate from our measurements, and recommend a change only if the trial supports it.”

Designing a trial requires no model access. Running one may require provider access, local compute, and permission to use the selected data. No accuracy, latency, or cost improvement is promised before a representative trial.

Created by Humans to know. This is an independent comparison workflow, not an endorsement by TypeSafe AI or any open-model author.
