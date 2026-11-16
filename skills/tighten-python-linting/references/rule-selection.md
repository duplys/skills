# Choosing tools and rules

Use this guide during assessment. These are candidates to investigate, not a universal configuration or an exhaustive catalog. Derive exact codes, settings, and availability from the chosen tool version.

## Choose tools by uncovered constraints

Keep existing tools when they can enforce the desired constraints. For a project without lint tooling, evaluate Ruff as a broad first candidate. Compare its actual enabled checks with existing Flake8 plugins, Pylint, security checks, and formatting conventions before suggesting consolidation. Similar rule names do not establish equivalent coverage.

Consider an additional tool only for a demonstrated gap:

- Pylint or a framework-aware plugin when its analysis catches relevant problems the proposed stack misses.
- A security analyzer such as Bandit when its selected checks add useful coverage beyond the existing security rule set. Neither linting nor a security scan proves the code secure.
- An import-boundary tool when documented package layering needs contracts beyond simple banned imports.
- The existing type checker, or a separately proposed one, when cross-function types and interface contracts need enforcement. Do not add a second checker without a specific benefit. Assess strictness and annotation debt independently from lint debt.

Tools that import project modules or load plugins may need more dependencies than syntax-based checks. Account for that environment and runtime cost in the proposal. Do not import application code merely to inventory the repository.

## Explore broad coverage, then justify exceptions

For Ruff, a temporary `ALL` trial can help discover candidates beyond familiar defaults. Treat the result as evidence to classify, not a configuration to commit unquestioningly. Some rules conflict with each other, chosen conventions, or the formatter. Inspect ignored and per-file-ignored rules too; broad selection does not necessarily override them. Report any deliberate suppression audit separately from the expected configured run.

Choose explicit rule codes or families when that makes the intended contract clearer. An `ALL` policy can also be proposed when its exceptions and upgrade behavior are understood and accepted. New tool releases may expand selectors; use the project's reproducibility mechanism and review rule changes on upgrades. Keep preview rules out unless there is a specific reason to propose their instability and maintenance cost.

## Candidate areas

Ruff prefixes below are discovery hints, not instructions to enable every rule in each family.

| Area | Candidates | Evidence and decisions to examine |
| --- | --- | --- |
| Correctness | `F`, `B`, selected `E`, `PL`, `RUF` | Undefined names, suspicious constructs, mutable defaults, and other concrete mistakes; check public re-exports and framework conventions. |
| Supported Python and imports | `UP`, `I`, `TID`, `ICN` | Lowest supported Python version, intentional import side effects, package structure, and established aliases. Do not modernize beyond runtime support. |
| Security and failures | `S`, `BLE`, `TRY`, `EM`, `DTZ` | Relevant dangerous APIs, exception behavior, and timezone conventions. Assess test assertions and deliberate exception handling individually. |
| Logging and debugging | `LOG`, `G`, `T10`, `T20` | Logging contracts, debugger remnants, and accidental prints; a CLI may intentionally print. |
| Async and framework behavior | `ASYNC` and applicable framework families | Libraries actually used, blocking work in async paths, and framework-specific semantics; avoid irrelevant families. |
| Tests | `PT` and relevant general checks | Test framework, fixture behavior, and assertion readability. Test exceptions should be specific rather than excluding tests wholesale. |
| Types | `ANN`, `TC`, and a type checker | Annotation policy, public interfaces, runtime annotation consumers, stubs, and strictness cost. Annotation linting does not establish type correctness. |
| Complexity and maintainability | `C90`, selected `PL`, `SIM`, `RET`, `ARG` | Complexity distribution and real maintenance problems. Thresholds should reflect a chosen constraint; flag likely refactors rather than applying them automatically. |
| Consistency and documentation | `N`, `D`, selected `E`, `W` | Existing conventions, public API needs, and formatter compatibility. Broad coverage can include style when it expresses a coherent agreed convention. |
| Suppression hygiene | `RUF100`, `PGH` | Stale or blanket suppressions and whether narrower, justified exceptions are possible. Do not remove necessary suppressions without assessing their purpose. |

For project-specific restrictions, look for a concrete convention worth encoding: disallow an obsolete API in favor of its supported wrapper, prevent a forbidden dependency, or enforce an established import alias. Name both the prohibited pattern and the permitted alternative. Base architecture restrictions on documented or user-confirmed boundaries; do not invent a layering policy from directory names alone. Prefer existing declarative capabilities to a bespoke plugin. Propose custom tooling only when simpler checks cannot express a valuable constraint.

## Assess migration cost and scope

Count violations by rule and affected files, then inspect examples to distinguish true defects, intentional patterns, and noise. Estimate effort qualitatively when findings do not support a precise estimate. High-volume mechanical edits can still create substantial review cost.

Distinguish production packages, tests, scripts, notebooks, migrations, and generated or vendored files. Scope rules to their purpose: generated output may warrant exclusion; authored migrations or tests may warrant only a few exceptions. Explain each proposed exclusion. Retain useful checks wherever possible.

For gradual typing, tighten the current checker where evidence supports it, possibly by module. Do not substitute blanket missing-import ignores, broad `Any` annotations, or blanket type ignores for an enforceable contract. See mypy's [existing-codebase guidance](https://mypy.readthedocs.io/en/stable/existing_code.html) when using mypy.

Every deferred rule should have a reason and an expansion condition, such as completing identified cleanup in a named package. A baseline must distinguish old findings from new ones; keeping only the total count constant can allow one defect to replace another. Validate the chosen mechanism instead of assuming a generic baseline flag exists.

## Tool documentation

Consult these when the local version's help is insufficient. Prefer documentation for the version being proposed.

- [Ruff rule catalog](https://docs.astral.sh/ruff/rules/): inspect individual rules, applicability, and fix availability.
- [Ruff linter](https://docs.astral.sh/ruff/linter/): selection, suppression, and fix behavior.
- [Ruff configuration](https://docs.astral.sh/ruff/configuration/): discovery, nested projects, and path resolution.
- [Ruff preview](https://docs.astral.sh/ruff/preview/): unstable behavior and opt-in rules.
- [Pylint usage](https://pylint.readthedocs.io/en/latest/user_guide/usage/run.html): invocation and configuration; choose the matching release documentation.
- [Bandit documentation](https://bandit.readthedocs.io/en/latest/): security checks and configuration.
