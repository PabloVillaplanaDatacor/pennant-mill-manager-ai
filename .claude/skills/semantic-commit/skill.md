---
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git add:*), Bash(git commit:*), Bash(git push:*)
description: "Generate semantic commit messages by analyzing changes, staging updates, and creating conventional commits automatically"
---

# Semantic Commit Generator

Generate well-formatted conventional commit messages by analyzing repository changes, then automatically stage, commit, and optionally push.

## What this command does

1. **Check Git Status**: Detect working tree and staged changes
2. **Stage Changes**: If nothing is staged, stage your current work (default: `git add .`)
3. **Analyze Changes**: Review the staged diff to understand what changed
4. **Categorize**: Determine commit type, scope, and description
5. **Generate Message**: Produce a conventional commit message
6. **Execute Commit**: Commit with the generated message
7. **Optional Push**: Ask whether to run `git push`

## Commit Types

- **feat**: New features or functionality
- **fix**: Bug fixes
- **docs**: Documentation changes
- **style**: Code formatting, missing semicolons, etc.
- **refactor**: Code restructuring without behavior changes
- **test**: Adding or updating tests
- **chore**: Maintenance tasks, dependency updates
- **perf**: Performance improvements
- **ci**: CI/CD pipeline changes

## Message Format

Follow conventional commits: `type(scope): description`

For complex changes, use bullet points:

```text
type(scope): brief description

- Detail 1
- Detail 2
- Detail 3
```

## Examples

### Feature Addition

```text
feat(auth): add social login integration

- Implement OAuth2 flow for Google and GitHub
- Add user profile synchronization
- Create callback handlers with error management
- Update user model for OAuth provider data
```

### Bug Fix

```text
fix(api): resolve race condition in session validation

Prevents concurrent session updates from causing authentication failures
```

### Refactoring

```text
refactor(components): restructure form validation logic

- Extract validation rules into reusable utilities
- Consolidate error handling across form components
- Remove duplicate validation code
- Improve type safety for validation rules
```

### Performance Improvement

```text
perf(database): optimize user search queries

Reduces query execution time from 2.3s to 45ms by adding proper indexing
```

### Documentation

```text
docs(api): update authentication endpoint documentation

Add comprehensive examples for OAuth2 flow and token refresh handling
```

### Testing

```text
test(auth): add comprehensive login flow test coverage

Includes success scenarios, validation errors, and edge cases
```

### CI/CD Changes

```text
ci(deploy): add automated database migration step

Ensures schema updates are applied before application deployment
```

### Style Changes

```text
style(components): apply consistent formatting and linting fixes

Resolves ESLint warnings and improves code readability
```

### Maintenance/Dependencies

```text
chore(deps): update React ecosystem to latest stable versions

Includes security patches and performance improvements
```

## Instructions

Execute these steps in order:

### 1) Check repo status

```bash
!git status
```

### 2) Ensure changes are staged

- If there are **already staged changes**, continue to the diff analysis step.
- If there are **no staged changes** but there are working tree changes, stage your current work:

```bash
!git add .
```

Then re-check:

```bash
!git status
```

> Tip: If you want more control, you can stage selectively with `git add -p` (interactive), but the default for this command is `git add .`.

If there are **still no staged changes**, stop and respond with:
- A short explanation that there is nothing to commit
- A suggestion to edit files or stage the correct ones
- Do **not** run `git commit`

### 3) Analyze the staged changes

```bash
!git diff --staged
```

### 4) Decide the commit message content

Based on the diff output, determine:
- Primary change type (feat, fix, docs, etc.)
- Affected scope/component (derive from paths/modules; keep it short)
- Description (present tense; first line under 72 characters)
- Whether to include bullet points (multi-part changes)

### 5) Commit automatically

```bash
!git commit -m "$(cat <<'EOF'
type(scope): description

Optional details:
- Point 1
- Point 2
EOF
)"
```

### 6) Confirm the commit

```bash
!git status
```

### 7) Ask to push (optional)

After a successful commit, ask the user:

- "Do you want me to run `git push` now? (yes/no)"

If the user says **yes**, run:

```bash
!git push
```

If the user says **no**, stop after the commit confirmation.

## Guidelines

- Focus on **what** changed and **why** it matters
- Use present tense ("add feature" not "added feature")
- Be specific about the impact or benefit
- Keep the first line under 72 characters
- Use bullet points for multi-part changes
- Maintain professional, descriptive language
- Include scope when changes affect specific components
- The commit message should clearly communicate the change without revealing the generation method

## Important Notes

- **Do NOT include** any mention of Claude Code, AI, or generation in commit messages
- **Do NOT add** footers like "Generated with Claude Code" or "Co-Authored-By: Claude"
- Keep commit messages clean and professional without revealing the automation
