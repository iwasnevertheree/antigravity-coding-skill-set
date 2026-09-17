# Workflow Walkthrough Example

Demonstrates the 5-phase professional coding framework on a task: **"Add input validation to profile update endpoint."**

---

## Scenario
- **Task**: The update endpoint accepts display name and email. Unhandled exceptions occur on invalid input. Add validation returning structured client errors.
- **Context**: RESTful service with existing routes, handlers, service layer, and test suite.

---

### Phase 1: Understand
1. **Requirements**: Reject empty or >50 char display names. Reject invalid emails. Return structured 400 errors.
2. **Change Impact**: Direct: handler; Consumers: HTTP clients; Contracts: 200 unchanged; invalid payloads receive 400 errors.
3. **Evidence**:
   - **Known**: Endpoints return JSON errors (verified in handlers).
   - **Inferred**: Validator belongs in domain module per layout.
   - **Assumed**: 50-char display name limit accepted (flagged in summary).

---

### Phase 2: Plan
1. **Tier**: Standard task (validation logic, callers, tests).
2. **Interface Compatibility**: Non-breaking for valid requests.
3. **Risk**: Domain trigger **Security** (untrusted input) → assessed impact: **Medium Risk** (boundary validation without schema mutation).
4. **Minimal Plan**: Add isolated validator helper; call before invoking service; add unit and integration tests.

---

### Phase 3: Implement
1. **Minimal Change**: Validate input before service call; avoid refactoring surrounding logic.
2. **Scope Firewall**: Adjacent `deleteProfile()` has broken indentation and log typo (`"faild"`). Per Scope Firewall, **leave untouched**; record in summary.

```text
function handleUpdateProfile(request, response):
    validationResult = validateProfileInput(request.body)
    if not validationResult.isValid:
        return response.status(400).json(validationResult.errors)
    return response.status(200).json(userService.updateProfile(request.userId, request.body))
```

---

### Phase 4: Verify
1. **Linters & formatters**: Style checks clean.
2. **Tests added**: Unit tests (valid input passes; empty/oversized name fails; malformed email fails); Integration tests (400 on invalid payload; 200 on valid).
3. **Feasible verification**: Local test runner passes (5 unit, 2 integration tests).
4. **Self-review**: Verified against 6 C's; zero edits outside `handleUpdateProfile` and tests.

---

### Phase 5: Document
1. **Commit Message**:
   ```text
   feat(user): add input validation to profile update endpoint

   Validate display name length and email formatting before updating
   user records, returning structured 400 errors on invalid inputs.
   ```
2. **User Summary**:
   - **What changed**: Input validation for display name and email in profile update handler.
   - **Verification**: Local test runner passed (5 unit, 2 integration tests).
   - **Declared assumption**: Display name limit set to 50 based on standard conventions.
   - **Observations / Discovered Technical Debt**: In profile handlers, `deleteProfile()` has inconsistent indentation and log typo (`"faild to delet"`). Left untouched per Scope Firewall.
