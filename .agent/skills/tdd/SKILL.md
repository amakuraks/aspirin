---
name: tdd
description: "Test-driven development with configurable strictness. Used in /work workflow."
---

# TDD Skill

## Cycle

```
1. Write failing test → 2. Run test (FAIL) → 3. Write minimal code → 4. Run test (PASS) → 5. Refactor
```

## Modes

| Mode | Behavior |
|------|----------|
| **strict** | Every task MUST have a test first. No exceptions. |
| **balanced** | Tests for logic/services/models. Skip for pure config, routing, views. |
| **relaxed** | Tests optional. Write when useful. |

Set via `tdd_mode` in `project-config.md`. Default: `balanced`.

## Laravel Test Patterns

```php
// Feature test (API endpoint)
public function test_user_can_create_transaction(): void
{
    $user = User::factory()->create();
    $response = $this->actingAs($user)->postJson('/api/transactions', [
        'amount' => 100.00,
        'description' => 'Test transaction',
    ]);
    $response->assertStatus(201)
             ->assertJsonStructure(['id', 'amount', 'description']);
    $this->assertDatabaseHas('transactions', ['amount' => 100.00]);
}

// Unit test (Service)
public function test_create_transaction_validates_balance(): void
{
    $service = new TransactionService();
    $this->expectException(InsufficientBalanceException::class);
    $service->create(['amount' => 99999, 'user_id' => 1]);
}
```

## React Test Patterns

```typescript
// Component test
test('renders transaction list', async () => {
  render(<TransactionList />);
  expect(await screen.findByText('Transaction 1')).toBeInTheDocument();
});

// Hook test
test('useTransactions fetches data', async () => {
  const { result } = renderHook(() => useTransactions());
  await waitFor(() => expect(result.current.loading).toBe(false));
  expect(result.current.transactions).toHaveLength(3);
});
```

## What to Test (balanced mode)

| Test | Skip |
|------|------|
| Service methods with business logic | Simple Eloquent CRUD with no logic |
| API endpoints (happy + error paths) | Route registration |
| Complex components with interaction | Static display components |
| Custom hooks with state management | Simple wrapper components |
| Validation rules (edge cases) | Config files |
