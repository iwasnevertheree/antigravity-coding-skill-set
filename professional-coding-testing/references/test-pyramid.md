# Test Pyramid Standards, AAA Pattern & Mocking Hygiene

Guidelines for test suite structure, pattern consistency, and boundary mocking rules.

---

## 1. Test Pyramid Balance

To maintain fast CI cycles and high confidence:
- **70% Unit Tests**: Fast (<10ms per test), isolated, in-memory. Test algorithmic branches, validation rules, edge cases, error conditions, and state transitions.
- **20% Integration Tests**: Verify collaboration across module boundaries, persistence repositories against test databases, and serialized contract responses.
- **10% End-to-End (E2E) Tests**: High-value smoke tests validating full workflow executions across actual runtime environments.

---

## 2. AAA Pattern Standards

Each test must be visually structured with clear Arrange, Act, Assert sections:

```python
def test_calculate_discount_applies_tier_two_rate():
    # Arrange: Set up inputs and expected conditions
    cart = ShoppingCart(user_id="user_123")
    cart.add_item(item_id="item_abc", price=100.0, quantity=2)
    discount_calculator = TierDiscountCalculator(tier_two_threshold=150.0, rate=0.15)

    # Act: Perform the single operation under test
    final_price = discount_calculator.apply_discount(cart)

    # Assert: Verify observable behavior
    assert final_price == 170.0
    assert cart.discount_applied == 30.0
```

### Assertion Hygiene Rules
- Assert specific values; avoid vague assertions like `assert result is not None` when `assert result.status == "ACTIVE"` is possible.
- Avoid multiple unrelated assertions per test; keep each test focused on one logical invariant.
- Provide descriptive failure messages for custom assertions.

---

## 3. Boundary Mocking Hygiene

### Where to Mock
- External HTTP / REST / RPC services.
- Real message queues, email servers, or third-party webhooks.
- System wall clocks (use mock clocks for deterministic time-based testing).
- File system access when testing high-volume logic that doesn't test file system semantics.

### Where NOT to Mock
- Internal pure business logic, calculations, or entity state.
- Data structures (lists, dicts, maps, domain models).
- The class or function under test.
- Tautological mocks: If mock setup requires 50 lines of boilerplate mocking internal private functions, the code under test violates Single Responsibility. Refactor to decouple dependencies rather than over-mocking.
