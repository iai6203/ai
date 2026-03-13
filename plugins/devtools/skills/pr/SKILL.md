---
name: pr
description: Analyze commits and create a pull request with a Conventional Commits title
allowed-tools: Bash(gh pr create *), Bash(gh repo view *), Bash(git log *), Bash(git branch *), Bash(git rev-parse *)
---

# PR Skill

Analyzes commits on the current branch and creates a pull request with an auto-generated Conventional Commits title.

## Execution Steps

0. Check if `gh` CLI is installed with `command -v gh`. If not found, notify the user to install it and abort.

1. Get the current branch with `git branch --show-current`. If the current branch is the same as the base branch, notify the user and abort.

2. Determine the base branch:
   - If an argument is provided (e.g., `/pr develop`), use it as the base branch
   - Otherwise, auto-detect using `gh repo view --json defaultBranchRef --jq '.defaultBranchRef.name'`

3. Get the commit list with `git log <base>..HEAD --oneline`. If there are no commits, notify the user and abort.

4. Analyze the commits and generate a PR title in Conventional Commits format: `<type>(<scope>): <subject>`

5. Show the generated title to the user and ask for confirmation. If the user requests changes, revise accordingly.

6. Run `gh pr create --base <base> --title "<title>" --fill` and display the resulting PR URL.

## Title Generation Rules

### Type

- Count the frequency of each type across all commits
- Select the most frequent type as the representative type
- Exception: if `feat` appears at least once, always use `feat`

### Scope

- If all commits share the same scope, use it as-is
- If scopes differ, use the most frequent one
- If the scope is ambiguous, use `custom`

### Subject

- Summarize the overall changes in Korean
- 50 characters or less
- If there is only one commit, use that commit message as the title directly

## Rules

- Write type and scope in English; write subject in Korean
- Use `--fill` flag to auto-populate the PR body from commit messages (compatible with non-interactive environments)
- Always show the generated title and get user confirmation before creating the PR
