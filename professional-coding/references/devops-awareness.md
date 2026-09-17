# DevOps Awareness

Principles for detecting and coordinating changes that intersect with build, packaging, containerization, and CI/CD pipelines.

---

## 1. When Does a Coding Change Intersect DevOps?

Check for DevOps implications whenever making changes that alter:
- **Dependencies**: Adding, removing, or updating external libraries.
- **Environment variables & configuration**: Introducing new settings, secrets, or configuration flags.
- **Entry points, ports, or processes**: Renaming executables, altering listen ports, or changing daemon lifecycles.
- **Filesystem & storage paths**: Modifying expected log locations, upload directories, or volume mounts.
- **Backing services**: Introducing or removing database, cache, or message broker requirements.

---

## 2. Repository Infrastructure Manifests

Look for these standard configuration points before committing:

| Category | Typical Manifests | What to Check |
|---|---|---|
| **Containers** | Containerfile, compose manifests | Base images, exposed ports, environment variables, build arguments |
| **CI/CD** | Pipeline workflows, CI configs | Test targets, lint commands, environment variables, secret names |
| **Configuration** | Environment templates, schemas, configs | Matching keys, default values, documentation for operators |
| **Package manifests** | Package definition files, lockfiles | Declared dependency versions, build scripts, engine requirements |

---

## 3. The 4-Step DevOps Coordination Pattern

1. **Detect**: When changing code, ask: *Does this alter how the application builds, runs, or configures?*
2. **Inspect**: Check relevant configuration files, compose manifests, or CI workflows in the repo.
3. **Propose**: Include necessary infrastructure adjustments in the same changeset or pull request.
4. **Scope boundary**: Make only minimal adjustments required for the task. Never rewrite pipelines or introduce new tools unless explicitly requested.

---

## 4. Common DevOps Pitfalls

- Adding an environment variable in code without updating environment templates or schemas.
- Adding a dependency without updating the lockfile.
- Changing an application port in source code without updating compose or container manifests.
- Adding a new test suite that is omitted from the CI pipeline workflow.
