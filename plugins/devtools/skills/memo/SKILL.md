---
name: memo
description: Summarize the current session's conversation into a structured note for future reference
allowed-tools: Bash(mkdir *), Bash(ls *), Bash(cat *), Read, Write, Glob
---

# Memo Skill

Summarizes the current conversation session into a structured note file, capturing key decisions, approaches, issues, and solutions for future reference when revisiting the same work.

## Storage

- Directory: `.notes/` in the project root (the repository's top-level directory)
- Filename format: `YYYY-MM-DD_<topic-slug>.md` (e.g., `2026-03-27_auth-refactor.md`)
- Topic slug: lowercase, hyphen-separated, derived from the main topic of the conversation
- If `.notes/` does not exist, create it
- If `.notes/.gitkeep` does not exist, create it so the directory can be tracked

## Execution Steps

1. Accept an optional argument as the topic hint (e.g., `/memo 인증 리팩토링`). If no argument is given, infer the topic from the conversation context.

2. Analyze the current conversation and extract:
   - **Topic**: what feature, bug, or task was discussed
   - **Context**: why this work was being done (background, motivation)
   - **Key decisions**: important choices made and their rationale
   - **Approach**: the technical approach or implementation strategy used
   - **Issues encountered**: problems, blockers, or gotchas discovered
   - **Solutions**: how the issues were resolved
   - **Remaining work**: any unfinished items or follow-ups identified
   - **Related files**: key files that were created or modified

3. Generate a structured memo in the format specified below.

4. Check if a memo with the same date and similar topic already exists in `.notes/`:
   - If it exists, ask the user whether to append to or overwrite the existing memo
   - If not, proceed to create a new file

5. Show the generated memo preview to the user and get confirmation before saving.

6. Write the memo file to `.notes/`.

## Memo Format

```markdown
# <Topic Title in Korean>

- **날짜**: YYYY-MM-DD
- **관련 파일**: `path/to/file1`, `path/to/file2`

## 배경

<Why this work was being done — 1-3 sentences>

## 핵심 결정사항

- <Decision 1 and its rationale>
- <Decision 2 and its rationale>

## 접근 방식

<Technical approach taken — concise description>

## 발생한 이슈

- <Issue 1>: <how it was resolved>
- <Issue 2>: <how it was resolved>

(Omit this section if no issues were encountered)

## 남은 작업

- [ ] <Follow-up item 1>
- [ ] <Follow-up item 2>

(Omit this section if nothing remains)
```

## Rules

- Write all memo content in Korean (except code identifiers, file paths, and technical terms)
- Be concise: summarize, do not transcribe the conversation verbatim
- Focus on information that would be useful when resuming this work in a future session
- Omit trivial or obvious details that can be derived from the code or git history
- If the conversation had no substantive technical content worth noting, inform the user and do not create an empty memo
- Do not include any auto-generated attribution text in the memo
