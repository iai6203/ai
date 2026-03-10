---
name: explain
description: Analyze staged changes and generate detailed per-file explanations
---

# Explain Skill

Analyzes staged changes and generates a structured, per-file explanation of what was changed and why.

## Execution Steps

1. Collect staged changes with `git diff --cached`
2. If there are no staged changes, notify the user and abort
3. Analyze each changed file for:
   - Change type (added/modified/deleted)
   - Summary of changes
   - Inferred intent/reason for the change
4. Output the per-file summary using the format below

## Output Format

```
## 변경 사항 요약

### `path/to/file.ts`
- **변경 유형**: 수정
- **변경 내용**: (구체적 설명)
- **변경 의도**: (추론된 이유)

### `path/to/another.ts`
- **변경 유형**: 추가
- **변경 내용**: (구체적 설명)
- **변경 의도**: (추론된 이유)
```

## Rules

- Write all explanations in Korean
- Keep technical terms and code identifiers in their original form
- Use relative file paths from the project root
