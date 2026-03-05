---
name: impact
description: Assess the impact of staged changes by analyzing scope, risk, affected areas, and breaking changes
---

# Impact Assessment Skill

Analyzes staged changes and produces an impact assessment report so developers can understand the blast radius before committing.

## Impact Dimensions

### 1. Scope of Change

| Level      | Description                                                  |
|------------|--------------------------------------------------------------|
| `trivial`  | Typos, comments, formatting — no logic change                |
| `small`    | Single function/method change in one file                    |
| `medium`   | Multiple functions or 2-5 files with related changes         |
| `large`    | Cross-module changes, new feature spanning many files        |
| `critical` | Core architecture, security, data model, or infra changes   |

### 2. Risk Level

| Level      | Criteria                                                      |
|------------|---------------------------------------------------------------|
| `low`      | Isolated change, no side effects expected                     |
| `moderate` | Touches shared code but limited blast radius                  |
| `high`     | Affects public API, shared state, or core business logic      |
| `critical` | Security-sensitive, data migration, or infra-level change     |

### 3. Affected Areas

- **Direct** — Files and modules explicitly modified
- **Downstream** — Consumers, dependents, or callers of changed code
- **Upstream** — Dependencies or providers that changed code relies on
- **Infrastructure** — Build, CI/CD, deployment, or configuration changes

### 4. Breaking Changes

Detect any of the following:

- Public API signature changes (parameters added/removed/reordered, return type changed)
- Database schema or data model changes
- Removed or renamed exports, classes, or public methods
- Environment variable or configuration key changes
- Protocol or contract changes between services

## Execution Steps

1. Run `git diff --cached` to retrieve staged changes
2. If there are no staged changes, notify the user and abort
3. Analyze each changed file and classify by impact dimension
4. Identify cross-file dependencies and downstream consumers where possible
5. Detect breaking changes by examining API surfaces, exports, and schemas
6. Produce the impact report in the format below

## Report Format

Output the report using this template:

```
## Impact Assessment

**Scope**: <level> — <one-line justification>
**Risk**: <level> — <one-line justification>

### Changed Files
- `path/to/file` — <brief description of change>

### Affected Areas
- **Direct**: <list>
- **Downstream**: <list or "None identified">
- **Upstream**: <list or "None identified">
- **Infrastructure**: <list or "None identified">

### Breaking Changes
- <description> (or "None detected")

### Recommendations
- <actionable suggestions: e.g., add tests, update docs, notify consumers>
```

## Rules

- **Read-only** — Do not modify, create, or delete any files
- Analyze only staged changes (`git diff --cached`), not unstaged or untracked files
- Be explicit about limitations — if the full dependency graph cannot be determined, say so
- Use concise bullet points; avoid lengthy prose
- Write the report in Korean, keep technical terms (file paths, identifiers) in their original form
