# Git Commit Message Prompt

Generate a commit message following this format:

```
type(scope): short description
```

## Types
- **feat**: New feature or functionality
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Code style/formatting changes (no logic changes)
- **refactor**: Code refactoring (no feature changes)
- **test**: Adding or updating tests
- **chore**: Maintenance tasks, build changes, etc.

## Scope
- Use the affected module/area: `api`, `web`, `db`, `config`, etc.
- For general changes, omit scope: `feat: add dark mode`
- Keep scope lowercase and descriptive

## Description
- Start with lowercase
- Be concise but descriptive
- Use imperative mood: "add", "fix", "update", not "added", "fixed", "updated"
- Max 50 characters preferred

## Examples
```
feat(api): add todo CRUD endpoints
fix(web): handle empty todo list state
docs: update README with setup instructions
style(web): format todo component with prettier
refactor(api): extract todo validation logic
test(api): add unit tests for todo service
chore: update dependencies
```

## Guidelines
- Focus on what changed, not how
- If multiple types, choose the most significant one
- For breaking changes, add `!` after type: `feat(api)!: change todo API structure`
- Keep messages under 72 characters total when possible