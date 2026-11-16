# Skills

A collection of agent skills, primarily for software engineering. Inspired by [Matt Pocock's skills repository](https://github.com/mattpocock/skills).

## Installation

Once a skill has been added and published to this repository, install it with:

```sh
npx skills add duplys/skills --skill <skill-name>
```

Omit `--skill <skill-name>` to choose from the available skills interactively. Available skills are listed below.

## Available skills

- [hypothesis-driven-development](skills/hypothesis-driven-development/SKILL.md) — make changes, verify features, or understand a codebase through testable hypotheses and bounded experiments.

  ```sh
  npx skills add duplys/skills --skill hypothesis-driven-development
  ```

- [interview-codebase](skills/interview-codebase/SKILL.md) — learn and check your understanding of a codebase or change through a guided interview.

  ```sh
  npx skills add duplys/skills --skill interview-codebase
  ```

- [tighten-python-linting](skills/tighten-python-linting/SKILL.md) — assess Python lint coverage, propose stricter checks, and implement the changes after agreement.

  ```sh
  npx skills add duplys/skills --skill tighten-python-linting
  ```

## Adding a skill

Create a directory under `skills/` for each skill, with a `SKILL.md` file containing YAML frontmatter and instructions:

```text
skills/
└── skill-name/
    └── SKILL.md
```

```markdown
---
name: skill-name
description: Describe what the skill does and when to use it.
---

# Skill name

Instructions for the agent.
```

Use the skill's name with `--skill` when installing it. Include any supporting files in the same directory, and keep each skill focused and independently usable.

## License

See [LICENSE](LICENSE).
