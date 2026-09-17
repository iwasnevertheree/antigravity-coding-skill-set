# Supply-Chain Security & Open Source License Compatibility

Guidelines for auditing dependency licenses, preventing typo-squatting, and assessing supply-chain vulnerabilities.

---

## 1. Open Source License Compatibility Matrix

| License Type | Permissible in Commercial Software? | Examples | Obligations & Restrictions |
|---|---|---|---|
| **Permissive** | **YES (Recommended)** | MIT, Apache 2.0, BSD-2/3-Clause, ISC | Retain original copyright notice; minimal restrictions. |
| **Weak Copyleft** | **Conditional** | LGPL, MPL 2.0, EPL | Modifications to the library itself must remain open; dynamic linking usually permissible. |
| **Strong Copyleft** | **HIGH RISK / ESCALATE** | GPL v2/v3, AGPL v3 | Requires entire derivative work/application source code to be released under the same copyleft license. Escalate to user before adding! |

---

## 2. Supply-Chain Threat Vectors

1. **Typo-Squatting**: Malicious packages registered with names nearly identical to popular libraries (e.g. `cross-env` vs `crossenv`). Always verify exact package names from official documentation.
2. **Dependency Confusion**: Internal private package names registered on public registries. Configure scoped registries (`@company/pkg`) to prevent public fallback.
3. **Compromised Maintainer Accounts**: Unvetted releases pushed by compromised developer credentials. Pin exact version numbers or hash integrity in lockfiles.
4. **Vulnerability Auditing**:
   - Run standard audit tools: `npm audit`, `pip-audit`, `cargo audit`, `trivy`.
   - Never suppress or ignore high/critical vulnerability warnings without explicit remediation plans.
