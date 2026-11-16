---
name: tighten-python-linting
description: Assess a Python codebase for additional linters and stricter lint rules, propose evidence-backed changes, and implement the agreed configuration and cleanup after user approval. Use when strengthening lint enforcement, especially for AI-assisted development.
---

# Tighten Python linting

Make more of the project's coding constraints executable so humans and coding agents receive fast, actionable feedback. Aim for the broadest useful rule coverage the project can enforce. Assess beyond a linter's defaults; rule count alone is not the goal. Rules should catch defects, enforce chosen conventions, or rule out unwanted designs with acceptable noise and maintenance cost.

This workflow has two phases: assess and propose, then implement the agreed changes. An assessment request authorizes investigation, not changes to the target project's dependencies, configuration, source, or enforcement. Keep analysis artifacts outside the project until implementation is authorized. If the user already approved a concrete proposal in this conversation, continue from that approval without asking again.

## Inspect the project and current enforcement

Determine the Python scope from the request and repository. In a mixed-language repository, assess the Python components and shared tooling that affects them. If there is no Python code, report that finding instead of installing Python tooling.

Inspect repository instructions and the current working tree, including relevant uncommitted files. Establish:

- Supported Python versions, package layout, source roots, tests, notebooks or scripts, and generated or vendored code.
- Dependency manager, development dependencies, lockfiles, and the commands contributors and CI actually use.
- Existing lint, formatting, type-checking, security, and architecture checks; their versions, configuration, enabled rules, exclusions, suppressions, and CI or hook integration.
- Project conventions and constraints visible in representative code: public APIs, framework patterns, dynamic behavior, imports, async code, logging, exceptions, and test style.

Follow configuration discovery and inheritance through nested projects. Do not assume a root configuration governs every file or that a declared tool runs in CI. Identify checks that exist but are bypassed, configured too narrowly, or allowed to fail.

Prefer strengthening the existing stack. Adding a tool needs a concrete coverage benefit; replacing one needs a migration comparison that accounts for plugins and checks the replacement does not cover. Treat type checkers and formatters as related, distinct checks rather than interchangeable linters.

## Evaluate broader coverage

Read [references/rule-selection.md](references/rule-selection.md) to evaluate candidate tools, rule families, and project-specific restrictions. Inspect the installed version's help and rule descriptions or version-matched official documentation before proposing exact settings. Do not assume current online defaults match a project's pinned version.

Run existing checks as a baseline when available. Then evaluate a broad candidate configuration, preferably across all relevant Python files. Use non-mutating modes and disable configured autofixes; for example, verify that Ruff runs with `--no-fix` even if the repository enables fixing by default. Direct caches and reports outside the project. Avoid dependency-manager commands that implicitly sync environments or rewrite lockfiles during assessment; use installed tools or an isolated environment within the available permissions. If a tool cannot run, distinguish source-based recommendations from measured findings.

Trial candidate settings in temporary configuration or an isolated copy. Preserve the current code, relative paths, and relevant configuration hierarchy, including uncommitted work; a snapshot of only committed files may assess the wrong code. An explicit config override can change path resolution or bypass nested configs, so account for those differences. Do not use fixes, formatting, or added suppressions to make the assessment pass.

Capture the commands, tool versions, checked paths, and results. Distinguish diagnostic failures from parser, configuration, dependency, and execution failures. Group findings by rule and affected files; inspect representative diagnostics to judge correctness and false positives. Separate mechanical cleanup from changes requiring a behavioral or API decision. If runtime or repository size forces sampling, label counts as sample counts and state what was not checked.

Classify candidates as:

- **Enable now:** Useful, compatible, and clean already or feasible with bounded cleanup.
- **Stage:** Useful, but requires substantial cleanup, annotations, or a narrower first enforcement scope.
- **Decline:** Redundant, incompatible, inapplicable, or too noisy for the project's actual patterns, with a specific reason.

Existing violations are a migration cost, not by themselves a reason to reject a valuable rule. Conversely, a zero-finding run does not prove a rule adds value. Explain the defect or unwanted design it would prevent. Preserve existing checks and do not silently widen ignores, remove rules, or raise complexity thresholds to achieve a green run.

## Present a concrete proposal and wait for agreement

State whether stronger enforcement is feasible now, feasible in stages, already sufficient, or unresolved because evidence is missing. A recommendation to make no additions is valid when the evidence supports it.

Make the proposal reviewable before asking the user to approve it. Include:

- The observed baseline and coverage gaps, with representative file references and measured counts where available.
- Proposed tools and versions, exact rule selections, thresholds, scope, and justified exceptions. Show the intended configuration snippets or patch, including how they merge with current settings.
- A concise rule table: rule or family, what it prevents, findings and cleanup cost, and enable/stage/decline decision. Cover meaningful rejected and deferred candidates as well as selected ones.
- Files to change: dependency and lockfiles, configuration, task commands, CI, existing hooks, contributor instructions, and any source or test cleanup. Explain any tool replacement or extra CI cost.
- The enforcement command and rollout: which paths fail immediately, how deferred paths become covered, and what cleanup or behavior changes approval includes.

For staged adoption, prefer clear package or path boundaries or a tool-supported baseline with a verified no-new-violations gate. Name the remaining debt and a concrete condition for expanding coverage. A warning-only job or a run that always exits successfully is observation, not enforcement. Explain blind spots in changed-files-only checks; do not present them as full-project coverage.

Ask the user to accept or amend this proposal, then wait before modifying the target project. Name this skill's requirement and link this `SKILL.md` when explaining the approval boundary. Do not treat silence as agreement. Incorporate requested additions or removals into the plan. Proceed if the user's response clearly authorizes the revised scope; clarify only a material unresolved choice. An addition phrased as a question is not blanket approval.

## Implement the agreed scope

Recheck relevant files if the project changed during review. Apply the accepted changes using the project's existing dependency manager, configuration style, and task conventions. Keep tools reproducible through the existing lockfile or pinning mechanism, and align versions used locally, by hooks, and by CI. Update lockfiles through the manager rather than inventing resolved entries. Do not replace the package manager or add a parallel environment system just for linting.

Wire the agreed checks into the normal developer command and existing CI so violations produce a failing exit status. CI should check rather than rewrite source files. Modify hooks when agreed; adding new hook infrastructure is a proposal item, not an automatic prerequisite.

Apply only the approved cleanup. Review autofix diffs, including changes labeled safe, for public re-exports, import side effects, framework behavior, and other semantic changes. Apply unsafe fixes or broad refactors only if covered by approval. Keep exceptions narrow, rule-specific, and justified; do not blanket-suppress findings, exclude authored code, or weaken assertions to satisfy the tool. If new findings require material behavior changes or a larger migration than agreed, finish independent approved work, present the additional change, and obtain agreement for that extension.

## Verify the enforcement and report

Run the actual contributor and CI check commands against the final configuration. Confirm that they discover the intended files, retain existing checks, and report no violations in the agreed enforced scope. Run relevant tests when cleanup can affect behavior and the project's required checks. Report pre-existing failures and unavailable checks separately from introduced failures.

In an isolated copy, introduce a small violation for a newly enabled rule in an included path and confirm that the real enforcement command fails for that rule. Remove the probe and confirm the clean command passes. For a staged baseline, also confirm that a new violation fails even when accepted legacy debt is present. This checks enforcement wiring; it does not require adding artificial violations or permanent tests to the repository.

Review the final diff for unrelated edits, lost rules, broad exclusions, dependency churn, and unintended formatting. Finish with the checks and rules added, changed files, commands and observed results, and any agreed deferred work or verification limits. Do not claim enforcement is complete if the gate fails or was not exercised.

## Inspiration

Inspired by Matt Pocock's [Why Software Fundamentals Still Matter](https://gitnation.com/contents/why-software-fundamentals-still-matter) and the motivating principle supplied with this skill: friction helps coding agents, and lint rules reduce the design space. The Python workflow and rule-selection guidance here are practical adaptations, not quotations or a prescribed Python toolchain from the talk.
