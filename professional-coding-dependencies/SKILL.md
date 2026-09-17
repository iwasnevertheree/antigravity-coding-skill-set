---
name: professional-coding-dependencies
description: >-
  Specialist skill for third-party library evaluation, 5-step dependency gate,
  package manifest management, lockfile integrity, version constraints, license
  compatibility, and supply-chain safety. Activate when adding, updating, removing,
  or auditing package dependencies or resolving dependency conflicts. Explicitly
  DO NOT activate when existing repository utilities or standard libraries already
  solve the requirement, or for trivial helpers.
---

# Professional Coding — Dependencies & Supply-Chain Specialist

**Domain**: Third-party library evaluation, 5-step dependency gate, package manifest management, lockfile integrity, version constraints, license compatibility, and supply-chain safety.

This specialist operates under the governance of the **Professional Coding Core** orchestrator (`professional-coding`). It enforces dependency discipline, **strictly prohibiting unnecessary packages when built-in capabilities suffice**.

---

## 1. Scope Boundaries (CAN / CANNOT / ESCALATE)

### CAN
- Evaluate candidate third-party libraries using the 5-step dependency decision gate.
- Inspect package manifests (`package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod`, `pom.xml`).
- Maintain lockfile integrity (`package-lock.json`, `Cargo.lock`, `poetry.lock`, `go.sum`).
- Audit open-source license compatibility (MIT, Apache-2.0, BSD vs. restrictive GPL/AGPL).
- Identify supply-chain risks, abandoned dependencies, or known CVE vulnerabilities.
- Resolve transitive dependency conflicts and semantic version pinning.

### CANNOT
- Add dependencies unnecessarily for trivial helpers (e.g., string padding, simple math).
- Add unmaintained, unvetted, or single-maintainer abandoned packages.
- Add packages with licenses incompatible with the repository or commercial requirements.
- Add heavy frameworks when existing project utilities or platform standard libraries suffice.
- Perform unreviewed major version upgrades that break backward compatibility.

### ESCALATE
- **STOP and escalate to the user** before introducing any heavy framework or major external dependency.
- Escalate when a candidate package carries non-permissive or ambiguous licensing terms.
- Escalate when resolving dependency version conflicts requires updating widespread transitive trees.

---

## 2. Activation Discipline

### Positive Activation Triggers
Activate when task requirements involve:
- Evaluating and adding a new third-party library or SDK to the project.
- Resolving dependency version conflicts, duplicate package versions, or peer dependency errors.
- Upgrading existing libraries or addressing security alerts (e.g. Dependabot, `npm audit`, `cargo audit`).
- Auditing project dependencies for open-source license compliance.
- Removing deprecated, dead, or redundant packages from manifests.
- **Complexity Alignment**: Typically **Standard** tier tasks.

### Negative Activation Exclusions
Explicitly **DO NOT** activate when:
- The platform standard library already provides the required capability (e.g. JSON parsing, basic cryptography, HTTP client).
- An existing utility module or helper function in the repository already solves the requirement.
- The task requires a trivial one-line algorithm easily implemented internally.
- *Alternative Route*: Handled internally by **Core** (Dependency Gate Tree 2).

---

## 3. Core Governance & Authority

1. **Orchestrator Precedence**: This specialist operates within the 5-phase meta-workflow (`Understand → Plan → Implement → Verify → Document`) governed by Core.
2. **Authority Hierarchy**:
   ```
   Priority 1 (Highest): User Intent (Explicit dependency preferences or constraints)
   Priority 2:          Repository Contract (Existing package managers, manifests, license rules)
   Priority 3:          Professional Coding Core (Scope Firewall, minimal change, safety)
   Priority 4:          Safety & Authorization Constraints (Absolute & Non-bypassable)
   Priority 5:          Dependencies Specialist (Evaluation gates & supply-chain safety)
   Priority 6 (Lowest):  Stylistic & Implementation Preferences
   ```
3. **Non-Bypassable Safety**: Dependency updates may never introduce known critical CVEs, bypass license verification, or download packages from unverified registries.

---

## 4. The 5-Step Dependency Gate

Before adding any new dependency to a repository, execute this evaluation gate:

```text
Need external functionality?
├── 1. Solved by internal repo utility? ──────► Use existing code
├── 2. Solved by declared dependency? ────────► Use declared dependency
├── 3. Solved by platform standard library? ──► Use standard library
├── 4. Eliminates substantial complex logic? (e.g., cryptographic ciphers, parsers)
│   ├── NO (trivial helper) ──────────────────► Implement internally
│   └── YES
└── 5. Mature, maintained, safely licensed?
    ├── YES ──► Add dependency; update lockfile & manifests
    └── NO  ──► Find reputable alternative or implement internally
```

---

## 5. Multi-Specialist Composition

Dependencies coordinates with other disciplines across package and build tasks:

- **Dependencies + Security (`professional-coding-security`)**: Dependencies evaluates package versions and provenance; Security audits vulnerability databases (CVEs), advisories, and permissions.
- **Dependencies + Testing (`professional-coding-testing`)**: Dependencies updates manifests and lockfiles; Testing runs automated test suites verifying clean builds and zero behavioral regressions.
- **Dependencies + Architecture (`professional-coding-architecture`)**: Architecture approves structural boundaries before introducing major frameworks or client SDKs.

---

## 6. Expected Inputs & Deliverables

| Category | Details |
|---|---|
| **Inputs** | Package manifests, lockfiles, candidate library names, vulnerability audit reports. |
| **Deliverables** | Updated manifests, synchronized lockfiles, dependency gate evaluation write-up, license audit summary. |
| **Verification** | Clean package installation (`npm ci`, `cargo build`, `pip install`), lockfile integrity checks, test suite pass. |

---

## 7. Reference Triggers

Consult detailed references on demand:
- [references/dependency-evaluation-gate.md](references/dependency-evaluation-gate.md): Detailed 5-step gate criteria, health metrics, and package vetting checklist.
- [references/supply-chain-license-safety.md](references/supply-chain-license-safety.md): License compatibility matrix (Permissive vs. Copyleft), typo-squatting, and lockfile hygiene.
