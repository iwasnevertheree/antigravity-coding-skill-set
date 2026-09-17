# Pull Request Description Template

Template for pull requests ensuring change impact, risk evaluation, verification feasibility, and scope firewall compliance are declared.

---

## Summary of Changes
- High-level overview of what this PR introduces and why.

## Change Impact & Risk Evaluation
- **Impact Scope**: [Internal component | Shared module | Public API / Contract]
- **Risk Level**: [Low | Medium | High] (assessed from actual impact, not just domain presence)
- **Interface Compatibility**: [Backward-compatible extension | Breaking change with updated callers]

## Evidence & Assumptions
- **Known Facts**: Verified in repository code or documentation.
- **Declared Assumptions**: Assumptions made during implementation.

## Scope Firewall Compliance
- [ ] Confirmed zero unrelated refactoring or style churn in modified files.
- **Observations / Discovered Technical Debt**: Unrelated issues or debt observed during implementation, left untouched for separate tracking.

## Verification Performed & Feasibility
- **Automated Tests Executed**: Test suites run and pass status.
- **Linters & Type Checkers**: Tools run and outcomes.
- **Unrunnable Checks & Confidence Limitations**: Any tests that could not run in the current environment and why.

## Checklist
- [ ] Tests added or updated for changed behavior
- [ ] Bug fixes include regression test
- [ ] Documentation updated if behavior changed
- [ ] No secrets, debug logs, or commented-out code in diff
