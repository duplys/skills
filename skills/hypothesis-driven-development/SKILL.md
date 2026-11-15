---
name: hypothesis-driven-development
description: Use hypothesis-driven development to make code changes, verify existing features, or understand a codebase through falsifiable claims, bounded experiments, and evidence-based decisions.
---

# Hypothesis-driven development

Treat proposed solutions and explanations of existing code as hypotheses. Start with the uncertainty that matters to the user's goal, choose evidence that could contradict the hypothesis, and run the smallest useful experiment before investing in a larger solution. End with what was learned and what that means for development.

## Choose the scope

Infer the mode from the request and inspect the relevant code, tests, documentation, and available observations. Use a concrete feature, change, or execution path as the initial scope; expand only when evidence calls for it.

| Mode | Question to resolve | Expected output |
| --- | --- | --- |
| Change | Will this intervention produce the intended outcome? | A minimal implementation or experiment, evidence, and a decision about further work. |
| Verify | Does the existing feature behave as expected under specified conditions? | Evidence supporting or contradicting the expectation, its coverage limits, and any justified follow-up. |
| Understand | Does this explanation accurately describe how the code works? | An evidence-backed explanation of responsibilities, control flow, or data flow, with remaining unknowns. |

Preserve the requested scope. Verification and understanding do not imply authorization to fix findings or refactor the application. Use isolated probes or temporary artifacts where needed. For an authorized change, carry the work through implementation and appropriate verification; do not stop after writing an experiment plan.

Separate established constraints from uncertain beliefs. Existing contracts, security boundaries, and explicit user requirements remain constraints; experiments test whether a proposed solution satisfies them. Distinguish documented intent, observed behavior, and inferred purpose. Code alone cannot establish what users need.

## Frame a testable hypothesis

Identify the current observation, the decision it creates, and the most consequential unknown. Prioritize the assumption whose failure would change the implementation or explanation. Split independent claims when they require different evidence.

Describe the hypothesis in terms of a capability or existing behavior, its predicted outcome in a stated context, and an observable signal. For example:

> Given [conditions], we expect [intervention or existing mechanism] to produce [outcome], because [reason]. We will assess this using [observable evidence].

Name a plausible alternative explanation and what observation would distinguish it. Avoid claims such as “the feature works” or “the architecture is clean.” State the actual behavior, affected users or inputs, and boundaries. Passing a test is evidence only if the test measures the intended outcome independently of the implementation.

Use a compact experiment note before executing the test. Keep it in the conversation for small tasks; use an existing project record when durable documentation is useful. Do not create paperwork for its own sake.

```text
Question and decision: What uncertainty matters, and what depends on it?
Observation: Current evidence and baseline, with source references.
Hypothesis: Predicted outcome, conditions, rationale, and alternative.
Experiment: Smallest discriminating probe, comparison, inputs, and evidence to collect.
Decision rules: Evidence for support, evidence for rejection, and inconclusive cases.
Bounds: Test scope or sample, observation window, effort limit, and relevant guardrails.
Next actions: What support, rejection, or an inconclusive result would each change.
```

## Define the experiment before the result

Choose the cheapest experiment that can resolve the selected uncertainty. Source tracing, a focused test, a reproduction, a benchmark, a prototype, or a user experiment may suffice; implementing the full feature is not the default experiment. Prefer existing observability and test infrastructure when they answer the question.

Make the decision rules specific enough to apply without reinterpretation:

- **Supported (provisionally accepted):** The stated evidence threshold is met within the defined scope and relevant guardrails hold.
- **Rejected:** A defined counterexample or sufficiently informative result contradicts the hypothesis or rules out the required effect.
- **Inconclusive:** Evidence is missing, conflicting, too noisy, too small, or the experiment is invalid. Failure to reach the support threshold alone does not necessarily justify rejection.

Match the evidence to the claim. A deterministic behavior claim may need representative cases and one decisive counterexample for rejection. A performance or product claim needs an explicit metric, baseline or control, meaningful effect threshold, and an observation window or sample appropriate to the decision. Predefine how uncertainty will be assessed; do not invent statistical confidence or pick thresholds after seeing results. Label proposed thresholds and assumptions when the user has not established them. If a missing business decision prevents interpretation, ask a focused question while continuing independent technical investigation.

For causal claims, control relevant differences between baseline and intervention. Record environmental conditions and likely confounders. Use counterexamples, boundary cases, or a competing mechanism to challenge the favored explanation. A mock may establish local behavior while leaving integration behavior unknown; a passing integration test cannot establish customer value.

Define a stopping point proportional to the experiment: completion of the selected cases, an agreed observation window, an effort limit, or a guardrail breach. Include rollback or cleanup for experiments that change state. Running local experiments does not itself authorize production changes, deployments, user exposure, or new telemetry collection.

## Run, observe, and revise

Capture the relevant baseline before intervening when a comparison is needed. For a defect fix, reproduce the problem first when practical. If reproduction is unavailable, state that limitation and use the strongest available alternative evidence.

In change mode, implement only what is needed to test the selected claim and fulfill the authorized request. Keep experiments reversible where practical. Check both the predicted outcome and relevant regressions. A successful narrow experiment can justify completing the requested implementation; avoid leaving an authorized task at prototype quality without explaining a genuine blocker.

In verify mode, compare existing behavior with the stated expectation and seek cases that could falsify it. Report a mismatch as a finding; proposing a fix does not authorize applying it.

In understand mode, trace concrete inputs through entry points, transformations, dependencies, and outputs. Use file references and executable probes where useful. Distinguish paths established by inspection from behavior actually observed at runtime. If execution is unavailable, bound conclusions to what the source and other evidence establish.

Record what was actually done: commands or inspection paths, inputs, relevant environment, measured results, and evidence locations. Do not describe a planned or blocked experiment as executed. Infrastructure failures generally leave the target hypothesis unresolved unless infrastructure behavior was the subject of the experiment.

Apply the original decision rules. Investigate contradictions rather than averaging them away. If results suggest a different hypothesis, preserve the original result, state the revision, and define a new experiment before evaluating it. Stop at the defined bound; do not keep adjusting the experiment until it supports the preferred answer.

## Turn learning into a development decision

For each material hypothesis, report the claim, experiment, observed evidence, verdict, limitations, and next action. Keep this concise for small tasks and include reproducible references when they matter. Distinguish technical correctness, operational behavior, and product outcomes; success in one does not establish the others.

- **Supported:** Explain what the evidence justifies retaining, implementing, or investigating next, and the conditions under which the conclusion applies.
- **Rejected:** Explain which assumption failed and whether the evidence favors a different design, a correction, retiring the idea, or revising the codebase explanation. Carry out follow-up changes only within the authorized scope.
- **Inconclusive:** Identify the missing evidence and the smallest useful next experiment. State which development decision remains unresolved; do not infer success or failure from lack of data.

Separate the status of the requested work from the status of the hypothesis. Code can be implemented and verified locally while a product-value hypothesis still awaits user evidence. An explanation can be supported for one path while other paths remain unknown. Prefer a bounded, useful conclusion over a claim of universal proof.

For examples of translating requests into experiments across the three modes, read [references/examples.md](references/examples.md) when helpful.

## Basis

This skill adapts the capability, expected outcome, and measurable evidence approach in Barry O'Reilly's [How to Implement Hypothesis-Driven Development](https://barryoreilly.com/explore/blog/how-to-implement-hypothesis-driven-development/). The codebase modes, experiment note, and decision rules here are practical adaptations, not quotations from *Lean Enterprise*.
