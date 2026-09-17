# Verification Feasibility, Scaling & Omission Reporting

Guidelines for scaling verification to risk, enforcing the bug-fix regression mandate, and disclosing unexecutable checks.

---

## 1. Risk-Scaled Verification Matrix

Verification must be proportional to blast radius and risk:

```text
Low Risk      ───►  Targeted Unit Tests + Existing Tests Pass + Linter
Medium Risk   ───►  Unit Tests + Boundary Integration Tests + Callers Verified + Linter
High Risk     ───►  Comprehensive Test Suite + Regression Tests + Edge Paths + Full Diff Review
```

### Low Risk Criteria
- Purely internal algorithmic edits with zero public contract changes.
- Localized bug fixes within a single private method.
- **Verification**: Run unit tests for changed function, verify existing test suite passes, run linter.

### Medium Risk Criteria
- Modifying service boundaries, internal API models, or adding non-breaking schema extensions.
- Updating shared utility modules consumed by multiple callers.
- **Verification**: Low risk checks + integration tests verifying calling contracts and error handling.

### High Risk Criteria
- Modifying authentication, authorization, or cryptography paths.
- Database migrations, data deletion paths, or concurrency/threading logic.
- Breaking public API changes or fundamental architectural restructuring.
- **Verification**: Exhaustive test execution, negative/adversarial test cases, regression test verification, and complete diff audit.

---

## 2. Bug Fix Regression Mandate

Every bug fix requires an automated regression test:
1. **The Test First Rule**: Before editing production code, author a test that reproduces the exact failure observed.
2. **Failure Verification**: Run the test and confirm it fails for the expected reason (reproducing the defect).
3. **Fix Application**: Apply the minimal correction to production code.
4. **Pass Verification**: Re-run the regression test and confirm it passes.
5. **No Regressions**: Run the broader existing test suite to ensure zero collateral damage.

---

## 3. Omission Disclosure Protocol

When environmental constraints prevent running automated checks:
- **Never pretend tests ran when they didn't.**
- Include an explicit section in the final task report:

```markdown
### Verification Performed & Limitations

#### Executed Checks
- [x] Unit test suite: `pytest tests/unit/test_order_pricing.py` (14 passed in 0.42s)
- [x] Static type check: `mypy src/pricing/` (0 errors reported)
- [x] Code linter: `ruff check src/pricing/` (All checks passed)

#### Omitted / Unexecutable Checks
- [ ] Integration test: `pytest tests/integration/test_payment_gateway.py`
  - **Reason**: Requires `STRIPE_TEST_SECRET_KEY` environment variable, which is not configured in this environment.
  - **Impact / Limitations**: End-to-end webhook handshake could not be validated against sandbox APIs.
  - **Recommended User Action**: Run `STRIPE_TEST_SECRET_KEY=sk_test_... pytest tests/integration/` in a configured staging environment.
```
