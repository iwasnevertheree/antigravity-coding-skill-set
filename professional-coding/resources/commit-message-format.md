# Conventional Commit Message Format

Specification for standardizing commit messages across projects.

---

## Structure

```text
<type>(<optional scope>): <description>

[optional body]

[optional footer(s)]
```

### Rules
- **Header**: Imperative, present tense, lowercase (e.g., "add", "fix", not "added"). No trailing period. Under 72 chars.
- **Body**: Explains motivation, context, and contrast with previous behavior. Wrapped at ~72 chars per line.
- **Footer**: References issues (`Closes #123`) or breaking change notices.

---

## Commit Types

| Type | When to Use | Example |
|---|---|---|
| `feat` | New feature or user-facing behavior | `feat(auth): add OAuth2 refresh token rotation` |
| `fix` | Bug fix or unexpected behavior patch | `fix(cart): resolve negative discount calculation` |
| `refactor` | Code restructuring without behavior changes | `refactor(parser): extract token scanning into helper` |
| `test` | Adding missing tests or correcting existing tests | `test(orders): add boundary cases for order total` |
| `docs` | Documentation updates only | `docs(readme): add environment setup commands` |
| `perf` | Performance or memory optimizations | `perf(search): cache query tokenization results` |
| `ci` | CI/CD, build script, or pipeline changes | `ci(github): add matrix testing across runtimes` |
| `chore` | Tooling config, dependency bumps | `chore(deps): update linter configuration` |

---

## Breaking Changes

Add `!` before the colon in header or include a `BREAKING CHANGE:` footer:

```text
feat(api)!: require api key header on all public endpoints

BREAKING CHANGE: The `apiKey` query parameter is no longer accepted.
```

---

## Examples

### Trivial Commit
```text
fix(typo): correct misnamed variable in connection pool
```

### Standard Commit
```text
fix(cart): prevent negative discount calculation on 100% coupon

Clamp percentage bounds between 0 and 100 before applying multiplier
to avoid producing negative order totals.

Closes #284
```
