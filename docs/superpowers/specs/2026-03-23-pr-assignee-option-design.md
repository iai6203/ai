# PR Skill `--assignee` Option Design

## Summary

Add an optional `--assignee` flag to the `/pr` skill so users can set assignees when creating a pull request.

## Current Behavior

- `/pr [base-branch]` — creates a PR with an auto-generated Conventional Commits title
- No way to specify assignees

## Proposed Behavior

- `/pr [base-branch] [--assignee <users>]`
- `--assignee` flag is optional; without it, behavior is unchanged
- When `--assignee user1,user2` is provided, use them directly
- When `--assignee` is provided without a value, fetch assignable users via `gh api` and present a numbered list for selection
- Parsing rule: `--assignee` must appear after the positional base-branch argument (if any). The token immediately following `--assignee` is always treated as a username list, never as a base branch.

## Changes

### File: `plugins/devtools/skills/pr/SKILL.md`

**allowed-tools:** Add `Bash(gh api *)` for fetching assignable users.

**Argument parsing (new Step 2.5, after base branch determination):**
- Parse `--assignee` flag from arguments
- If flag is present with value: store the comma-separated assignee list
- If flag is present without value: fetch assignable users with `gh api repos/{owner}/{repo}/assignees --jq '.[].login' | head -20` and present a numbered list. The user selects by number or types usernames directly.
- If flag is absent: skip assignee flow entirely
- If fetching assignable users fails (403, 404, etc.), notify the user and proceed without assignees rather than aborting

**Confirmation preview (Step 5):**
- Show assignees in the preview when set:
  ```
  Title: <title>
  Assignees: user1, user2
  ```

**PR creation (Step 6):**
- Append `--assignee user1 --assignee user2` (one flag per user) to `gh pr create` command when assignees are set
- The `--fill` flag remains unchanged and works alongside `--assignee`

### Pattern Reference

The implementation follows the same pattern as `plugins/devtools/skills/issue/SKILL.md`, which already handles assignee fetching via `gh api`.

## Non-Goals

- No default assignee (e.g., auto-assigning self)
- No reviewer assignment (`--reviewer` is a separate concern)
- No persistent assignee configuration
