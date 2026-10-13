# Commit Message Convention

This project follows [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

## Format

```
<type>(<scope>): <short summary>

[optional body]

[optional footer(s)]
```

## Types

| Type | When to use |
|------|-------------|
| `feat` | Adding a new skill, rule, or plugin support |
| `fix` | Fixing incorrect principle, bad example, or typo |
| `docs` | README, CONTRIBUTING, or non-skill documentation |
| `refactor` | Restructuring skill content without changing meaning |
| `chore` | Package.json, CI, tooling, non-content changes |
| `release` | Version bump |

## Scopes

| Scope | Examples |
|-------|---------|
| `skill/<name>` | `skill/solid-principles`, `skill/clean-code` |
| `rules` | Changes to always-on rules |
| `plugin` | plugin.json, platform manifests |
| `npm` | package.json, publishing config |
| `ci` | GitHub Actions, workflows |

## Examples

```
feat(skill/solid-principles): add ISP interface segregation example

fix(skill/clean-code): correct function parameter count rule (≤3, not ≤4)

docs(readme): add Windsurf installation instructions

feat(rules): add no-magic-numbers rule to core-principles

chore(npm): update package.json homepage url

feat(plugin): add Kimi plugin manifest

release: bump version to 1.1.0
```

## Rules

- **Summary line**: max 72 characters, imperative mood ("add", not "added" or "adds")
- **Body**: explain *why*, not *what* — the diff shows what
- **Breaking changes**: add `!` after type/scope and `BREAKING CHANGE:` in footer
- **Reference issues**: `Closes #123` or `Refs #123` in footer
