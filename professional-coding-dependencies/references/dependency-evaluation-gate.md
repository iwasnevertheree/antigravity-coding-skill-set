# The 5-Step Dependency Evaluation Gate

Detailed evaluation rubric for determining whether to add a new third-party dependency.

---

## 1. Evaluation Criteria Checklist

Before adding a third-party package, evaluate each dimension:

1. **Internal Solution Check**:
   - Search existing repository code for identical or similar implementations.
   - If an internal utility already exists, reuse it.
2. **Platform Capability Check**:
   - Can this be accomplished in under 20 lines of clean code using the language standard library?
   - If yes, implement internally.
3. **Ecosystem & Health Metrics**:
   - **Recent Activity**: Has the repository been updated within the last 6 months?
   - **Community Adoption**: Is the package widely adopted (>100k downloads/month or equivalent)?
   - **Issue Responsiveness**: Are reported security issues addressed promptly?
4. **Maintenance Overhead**:
   - What is the transitive dependency footprint? (Does adding 1 package pull in 80 sub-dependencies?)
   - Does it support modern runtime versions (e.g. Node LTS, Python 3.10+, Go current)?

---

## 2. Lockfile Hygiene

- **Always Commit Lockfiles**: Lockfiles (`package-lock.json`, `poetry.lock`, `Cargo.lock`) guarantee deterministic, reproducible builds across development and CI.
- **Never Manually Edit Lockfiles**: Always allow the package manager CLI to generate and update lockfiles.
- **Strict Verification**: In CI pipelines, use reproducible install commands (e.g. `npm ci`, `poetry install --frozen`, `cargo check --locked`).
