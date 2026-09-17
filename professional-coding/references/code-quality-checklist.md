# Code Quality Checklist

Pre-commit quality gate checklist to verify before completing any coding task.

---

## Pre-Commit Quality Gate

### 1. Correctness & Safety
- [ ] Requirements fully satisfied without regressions
- [ ] Edge cases handled (empty collections, boundary limits, null/nil states)
- [ ] Errors handled explicitly using project-idiomatic patterns; no swallowed errors
- [ ] No hardcoded credentials, API keys, or environment-specific secrets
- [ ] Shared mutable state protected against race conditions

### 2. Minimal Change & Scope Firewall
- [ ] Smallest change fully satisfying requirements
- [ ] Scope Firewall enforced: zero opportunistic refactoring or reformatting of untouched code
- [ ] Adjacent defects left in place and logged under *Observations / Discovered Technical Debt*
- [ ] No debug prints, console logs, or commented-out code blocks left in diff

### 3. Code Style & Consistency
- [ ] Follows project conventions, naming standards, and directory patterns
- [ ] Adheres to configured linter, formatter, and type-checker rules
- [ ] Functions remain small, focused, and single-responsibility
- [ ] Inline comments explain *why*, not *what*

### 4. Testing & Verification
- [ ] Broadest relevant verification available and feasible executed
- [ ] Automated tests cover new logic (happy and error paths)
- [ ] Bug fixes include regression test reproducing defect before fix
- [ ] Unrunnable verification checks documented with confidence limitations explained

### 5. Documentation & Git Safety
- [ ] Public contracts, interfaces, and configs documented if modified
- [ ] Conventional commit message drafted per project standards
- [ ] `git status` and `git diff` clean with only intended modifications
- [ ] No destructive operations or unapproved commits executed
