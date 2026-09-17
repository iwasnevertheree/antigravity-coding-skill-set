# Pre-Commit Diff Audit Checklist & Review Template

Step-by-step audit workflow for pre-commit verification and PR review reports.

---

## 1. Pre-Commit Diff Checklist

Before creating a commit or opening a pull request, verify each item:

- [ ] **Minimal Change**: Every changed line directly supports the requested task.
- [ ] **No Debug Leftovers**: Zero `console.log`, `print()`, `debugger`, or temporary diagnostics remain.
- [ ] **No Commented-Out Code**: Dead code has been deleted, not commented out.
- [ ] **No Formatting Churn**: Untouched lines have not been reformatted.
- [ ] **No Secret Leaks**: No API keys, passwords, or personal credentials exist in diff.
- [ ] **Scope Firewall**: Unrelated defects discovered during work are logged under *Observations / Discovered Technical Debt* rather than touched in code.
- [ ] **Tests Pass**: All relevant unit and integration test suites pass cleanly.

---

## 2. Structured Code Review Report Template

When performing a formal code review or PR audit, format findings as follows:

```markdown
### Code Review Summary

* **Verdict**: [Approve | Request Changes | Comment]
* **Scope Adherence**: [Clean Minimal Change | Scope Creep Detected]

#### 6 C's Assessment
- **Correctness**: [Pass / Issues identified]
- **Clarity**: [Pass / Naming or explanation improvements recommended]
- **Consistency**: [Pass / Conforms to repository patterns]
- **Completeness**: [Pass / Tests and docs verified]
- **Conformance**: [Pass / Linters and security checks green]
- **Constraint**: [Pass / Minimal changeset confirmed]

#### Detailed Findings
1. **[Finding Title]** (`path/to/file.ext:line`)
   - **Issue**: Explanation of concern.
   - **Recommendation**: Suggested remediation.

#### Discovered Technical Debt (Out of Scope)
- Note neighboring debt observed during review for future triage.
```
