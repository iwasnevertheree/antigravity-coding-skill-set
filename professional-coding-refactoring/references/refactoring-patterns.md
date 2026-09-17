# Common Refactoring Patterns & Code Smell Remediation

Catalog of disciplined transformations that improve internal code quality without altering external behavior.

---

## 1. Primary Refactoring Patterns

### 1. Extract Function / Method
- **Problem**: A method is too long (>40 lines) or performs multiple tasks.
- **Remediation**: Isolate a cohesive block of code, turn it into a dedicated helper function, and name it to explain *what* it does.

### 2. Replace Nested Conditionals with Guard Clauses
- **Problem**: Deeply nested `if/else` ladders obscure the happy path.
- **Remediation**: Check error or exit conditions at the top of the function and return immediately:
  ```python
  # Before
  def process(order):
      if order is not None:
          if order.is_valid():
              if not order.is_cancelled():
                  execute(order)

  # After (Guard Clauses)
  def process(order):
      if order is None or not order.is_valid():
          return
      if order.is_cancelled():
          return
      execute(order)
  ```

### 3. Decompose God Class into Collaborators
- **Problem**: A class accumulates 1,000+ lines, knowing too much and doing too much.
- **Remediation**: Identify subset of methods operating on specific state fields; extract those fields and methods into a collaborator class injected via constructor.

### 4. Replace Primitive Obsession with Domain Value Objects
- **Problem**: Passing loose strings or numbers representing domain concepts (e.g., raw string for email or phone number).
- **Remediation**: Wrap the primitive in a strongly typed domain object that encapsulates validation rules.
