# PR Skill `--assignee` Option Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add `--assignee` flag support to the `/pr` skill for setting PR assignees at creation time.

**Architecture:** Single-file edit to `plugins/devtools/skills/pr/SKILL.md`. Add `gh api` to allowed-tools, insert assignee parsing/selection steps, update confirmation preview and PR creation command.

**Tech Stack:** Markdown skill definition, `gh` CLI

**Spec:** `docs/superpowers/specs/2026-03-23-pr-assignee-option-design.md`

---

### Task 1: Update allowed-tools

**Files:**
- Modify: `plugins/devtools/skills/pr/SKILL.md:4`

- [ ] **Step 1: Add `Bash(gh api *)` to allowed-tools**

Update the frontmatter `allowed-tools` line to include `Bash(gh api *)`:

```
allowed-tools: Bash(gh pr create *), Bash(gh repo view *), Bash(git log *), Bash(git branch *), Bash(git rev-parse *), Bash(gh api *)
```

- [ ] **Step 2: Commit**

```bash
git add plugins/devtools/skills/pr/SKILL.md
git commit -m "feat(skill): PR 스킬 allowed-tools에 gh api 추가"
```

---

### Task 2: Add argument parsing for `--assignee`

**Files:**
- Modify: `plugins/devtools/skills/pr/SKILL.md:17-19`

- [ ] **Step 1: Update Step 2 to clarify base branch + assignee parsing**

Keep base branch determination as Step 2 and add a new Step 2.5 for assignee parsing. The key parsing rule: `--assignee` must appear after the positional base-branch argument (if any). The token immediately following `--assignee` is always treated as a username list, never as a base branch.

Replace the current Step 2:

```markdown
2. Determine the base branch:
   - If a positional argument is provided before any `--` flags (e.g., `/pr develop ...`), use it as the base branch
   - Otherwise, auto-detect using `gh repo view --json defaultBranchRef --jq '.defaultBranchRef.name'`

2.5. Parse the `--assignee` flag (if present):
   - `--assignee` must appear after the base branch argument (if any). The token immediately following `--assignee` is always treated as a username list, never as a base branch.
   - If `--assignee <users>` is provided (e.g., `--assignee user1,user2`), store the comma-separated usernames
   - If `--assignee` is provided without a value, proceed to Step 3
   - If the flag is absent entirely, skip Step 3
```

- [ ] **Step 2: Commit**

```bash
git add plugins/devtools/skills/pr/SKILL.md
git commit -m "feat(skill): PR 스킬 인자 파싱에 --assignee 플래그 추가"
```

---

### Task 3: Add assignee selection step

**Files:**
- Modify: `plugins/devtools/skills/pr/SKILL.md` (insert new step after Step 2)

- [ ] **Step 1: Insert new Step 3 for interactive assignee selection**

Add a new step between the current Step 2 (argument parsing) and Step 3 (commit list). Renumber subsequent steps accordingly.

```markdown
3. (Only when `--assignee` was provided without a value) Fetch assignable users and present selection:
   - Run `gh api repos/{owner}/{repo}/assignees --jq '.[].login' | head -20`
   - If the fetch fails (403, 404, network error), notify the user and proceed without assignees
   - Present a numbered list of available users
   - The user selects by number(s) or types username(s) directly
```

- [ ] **Step 2: Renumber all subsequent steps** (old 3→4, 4→5, 5→6, 6→7)

- [ ] **Step 3: Commit**

```bash
git add plugins/devtools/skills/pr/SKILL.md
git commit -m "feat(skill): PR 스킬에 assignee 선택 단계 추가"
```

---

### Task 4: Update confirmation preview and PR creation

**Files:**
- Modify: `plugins/devtools/skills/pr/SKILL.md` (Steps 6 and 7 after renumbering)

- [ ] **Step 1: Update confirmation step to show assignees**

Update the confirmation step (now Step 6) to include assignee information when set:

```markdown
6. Show the generated title (and assignees if set) to the user and ask for confirmation:
   ```
   Title: <title>
   Assignees: <assignees or (none)>
   ```
   If the user requests changes, revise accordingly.
```

- [ ] **Step 2: Update PR creation step to include `--assignee` flags**

Update the creation step (now Step 7):

```markdown
7. Run `gh pr create --base <base> --title "<title>" --fill` with additional flags:
   - Add `--assignee <user>` for each assignee (one flag per user, e.g., `--assignee user1 --assignee user2`)
   - Display the resulting PR URL.
```

- [ ] **Step 3: Commit**

```bash
git add plugins/devtools/skills/pr/SKILL.md
git commit -m "feat(skill): PR 스킬 확인 프리뷰 및 생성 명령에 assignee 반영"
```

---

### Task 5: Final review and squash

- [ ] **Step 1: Read the complete SKILL.md and verify**

Verify the full file reads correctly: steps are numbered sequentially, allowed-tools is correct, and the `--assignee` flow is coherent end-to-end.

- [ ] **Step 2: Squash implementation commits**

Use `/squash` to combine the implementation commits into a single commit.
