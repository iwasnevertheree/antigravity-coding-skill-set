# Change Impact Analysis

Guidance for determining blast radius and consumer dependencies before modifying components.

---

## 1. The 7 Impact Dimensions

Before modifying an existing component, systematically evaluate:

1. **Direct Component Changes**: What routines, structures, types, or internal logic are directly modified?
2. **Consumers & Dependents**: What internal modules, external services, or pipelines call or consume this component?
3. **Interfaces & Contracts**: Does this change alter public signatures, schemas, serialization formats, API routes, or CLI flags?
4. **Associated Tests**: What existing unit, integration, and end-to-end tests exercise this component and its callers?
5. **Configuration & Generated Artifacts**: Are package manifests, environment definitions, lockfiles, or generated files affected?
6. **Side Effects & State**: Does this alteration impact shared mutable state, cache validity, background queues, or concurrency?
7. **Verification Scope**: What is the minimal sufficient set of feasible tests and checks needed to confirm correctness?

---

## 2. Proportional Depth by Complexity Tier

Scale analysis depth to the risk and tier of the change:

- **Trivial Tier**: Instantaneous check verifying internal edits touching zero public/shared interfaces.
- **Standard Tier**: Immediate boundary check tracing callers, consumers, and related tests. Confirm backward compatibility.
- **Complex Tier**: Comprehensive mapping across multi-layer consumers, contracts, schemas, and deployment artifacts. Include rollback considerations.

---

## 3. Tracing Callers and Consumers (Language-Agnostic)

Use project-appropriate search techniques to identify consumers:
- **Symbol Search**: Query repository for exact occurrences of the identifier being modified.
- **Import / Reference Scan**: Locate modules that import, include, or register the target component.
- **Contract & Schema References**: Inspect route registries, schemas, serialization definitions, and test fixtures.
- **Test Coverage Inspection**: Identify test suites that invoke the component directly or through higher integrations.

---

## 4. Scoping Verification from Impact

- **Internal change (zero interface changes)**: Unit tests covering changed logic and relevant existing tests.
- **Shared internal interfaces**: Unit tests, direct caller tests, and interface integration checks.
- **Public contracts / data persistence**: Broadest feasible verification across direct consumers and contract suites.
