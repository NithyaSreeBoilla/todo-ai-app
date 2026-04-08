# Pull Request Review Prompt

Review this pull request systematically, focusing on architecture, code quality, error handling, and security. Provide a structured review with issues, suggestions, and a final verdict.

## Review Checklist

### Architecture Issues
- [ ] Does the code follow the established architecture patterns?
- [ ] Is there proper separation of concerns (routes → services → repositories)?
- [ ] Are dependencies injected correctly?
- [ ] Does it maintain consistency with existing codebase structure?
- [ ] Are there any circular dependencies or tight coupling?

### Code Quality
- [ ] Are type hints used consistently (Python)?
- [ ] Are there proper docstrings/comments?
- [ ] Is the code DRY (Don't Repeat Yourself)?
- [ ] Are variable/function names descriptive and consistent?
- [ ] Is the code properly formatted and linted?
- [ ] Are there any unused imports or variables?

### Error Handling
- [ ] Are exceptions handled appropriately?
- [ ] Are custom business logic exceptions defined?
- [ ] Is error logging implemented?
- [ ] Are HTTP status codes used correctly?
- [ ] Is user input validated properly?
- [ ] Are edge cases considered?

### Security Risks
- [ ] Is user input sanitized/validated?
- [ ] Are there any SQL injection vulnerabilities?
- [ ] Is authentication/authorization handled properly?
- [ ] Are sensitive data (passwords, tokens) handled securely?
- [ ] Are there any hardcoded secrets or credentials?
- [ ] Is CORS configured appropriately?

## Review Output Format

### Issues
List any problems found, categorized by type:

**Architecture Issues:**
- Issue 1 with specific details
- Issue 2 with code references

**Code Quality Issues:**
- Issue 1
- Issue 2

**Error Handling Issues:**
- Issue 1
- Issue 2

**Security Issues:**
- Issue 1 (mark critical issues clearly)

### Suggestions
Provide actionable improvements:

**Architecture Improvements:**
- Suggestion 1 with reasoning
- Suggestion 2

**Code Quality Improvements:**
- Suggestion 1
- Suggestion 2

**Error Handling Improvements:**
- Suggestion 1
- Suggestion 2

**Security Enhancements:**
- Suggestion 1
- Suggestion 2

### Final Verdict
Choose one:
- **✅ Approve**: No blocking issues, ready to merge
- **🔄 Request Changes**: Address the issues before merging
- **❌ Reject**: Fundamental problems that require major rework

**Rationale:** Brief explanation of the verdict

## Review Guidelines
- Be specific: Reference file names, line numbers, and code snippets
- Prioritize: Focus on critical issues first (security, architecture)
- Be constructive: Explain why changes are needed
- Consider impact: Think about maintainability, performance, and scalability
- Check tests: Ensure adequate test coverage for new/changed code
- Verify functionality: Does the code actually work as intended?