# Jev Workflow Design

Help your agent identify one decision where Jev could improve your existing workflow, define the evidence and possible answers, and plan a comparison with rules or your current LLM. If Jev adds no value, the skill should say so.

## Use

Load this folder with your agent’s supported skill mechanism, including [SKILL.md](SKILL.md) and its optional [worked example](references/decision-example.md).

Ask: “Look at my workflow. Find one decision where Jev could help, define the inputs and fallback, and show how we would test it against what I do today. If it adds no value, tell me.”

No API key is needed for the design stage. Running Jev requires TypeSafe access and authorization to send the selected data.

Created by Humans to know. Jev is developed by TypeSafe AI; this independent workflow is not endorsed by TypeSafe. The worked example is illustrative, and no live Jev accuracy, latency or cost has been measured.
