# Evidence-Based Root-Cause Analysis & Diagnostic Hygiene

Guidelines for scientific defect isolation, stack trace inspection, and temporary diagnostics protocol.

---

## 1. Distinguishing Symptom vs. Root Cause

| Failure | Surface Symptom (Crash Site) | Underlying Root Cause |
|---|---|---|
| `NullPointerException` / `TypeError: cannot read property of undefined` | Line where `user.profile.name` is accessed. | Upstream database query returned an empty record because user was soft-deleted, but caller did not handle null case. |
| `IndexOutOfBoundsException` | Loop accessing array at index `i`. | Loop counter initialized with `<=` instead of `<`, or off-by-one pagination slice upstream. |
| `Deadlock` / `Timeout` | Thread blocked on `mutex.lock()`. | Reverse lock acquisition order between two competing workers in separate modules. |

**Rule**: Never patch the crash site with a superficial null-check or dummy return if the invalid state should have been prevented or handled systematically upstream.

---

## 2. Scientific Hypothesis Testing

When the cause is non-obvious:
1. **Observe**: Collect raw logs, error codes, and caller parameters.
2. **Hypothesize**: "I hypothesize that function X receives parameter Y formatted as ISO string instead of epoch timestamp."
3. **Test Hypothesis**: Check actual input value using existing tests or targeted inspection.
4. **Validate / Reject**: If true, design minimal fix; if false, formulate new hypothesis based on evidence.

---

## 3. Temporary Diagnostics Protocol

When dynamic inspection is needed to observe runtime state:
1. **Permissible**:
   - Adding targeted temporary log statements or print statements showing variable state.
   - Inserting assertions to detect when state becomes corrupted.
2. **Hygiene Rules**:
   - Keep temporary diagnostic statements minimal and localized.
   - Never commit temporary diagnostics to source control.
   - **Mandatory Diff Audit**: Before declaring the fix verified or requesting commit, inspect `git diff` to ensure 100% of diagnostic logs and print statements are completely removed.
