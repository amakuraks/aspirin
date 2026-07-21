---
name: architecture-check
description: "Validate layered architecture for Laravel and React. Used by /gate workflow."
---

# Architecture Check Skill

## Purpose

Verify that the plan and code follow proper layered architecture. Prevents spaghetti code, God classes, and layer violations.

## Laravel Architecture

### Layer Rules

```
Route → Controller → FormRequest → Service → Model/Repository → Database
         (thin)      (validation)   (logic)   (data access)
```

| Layer | Responsibility | Must NOT contain |
|-------|---------------|-----------------|
| **Route** | URL → Controller mapping | Logic, validation |
| **Controller** | Receive request, delegate to service, return response | Business logic, direct DB queries, complex conditionals |
| **FormRequest** | Input validation rules | Business logic, DB queries |
| **Service** | Business logic, orchestration | HTTP concerns (`request()`, `response()`), direct `DB::` calls for simple ops |
| **Model** | Relationships, scopes, accessors, casts | Business logic, HTTP concerns |
| **Repository** | Complex queries (optional for simple Eloquent) | Business logic, HTTP concerns |

### Violations to Flag

| Violation | Severity | Example |
|-----------|----------|---------|
| Business logic in controller | P2 | `if ($user->balance < $amount) { ... }` in controller |
| Direct DB queries in controller | P2 | `DB::table('users')->where(...)` in controller |
| HTTP concerns in service | P2 | `$request->input()` in service method |
| Validation in controller | P3 | Manual `$request->validate()` instead of FormRequest |
| Fat model (business logic) | P2 | Complex business rules in model methods |
| Controller > 100 lines | P2 | Controller doing too much |
| Service > 200 lines | P3 | Consider splitting into focused services |

### Correct Pattern

```php
// Route
Route::post('/transactions', [TransactionController::class, 'store']);

// Controller (thin — delegates)
public function store(StoreTransactionRequest $request)
{
    $transaction = $this->transactionService->create($request->validated());
    return response()->json($transaction, 201);
}

// FormRequest (validation)
public function rules(): array
{
    return ['amount' => 'required|numeric|min:0.01', ...];
}

// Service (business logic)
public function create(array $data): Transaction
{
    // Business rules here
    return Transaction::create($data);
}
```

---

## React Architecture

### Layer Rules

```
Page → Component → Hook → API Client → TypeScript Interface
(composition) (UI)  (logic) (HTTP)     (types)
```

| Layer | Responsibility | Must NOT contain |
|-------|---------------|-----------------|
| **Page** | Compose components, route-level layout | Complex logic, direct API calls |
| **Component** | UI rendering, user interaction | API calls, complex business logic |
| **Hook** | Reusable stateful logic, data fetching | UI rendering, direct HTTP calls |
| **API Client** | HTTP abstraction (axios/fetch wrappers) | UI logic, state management |
| **Interface/Type** | Data shape definitions | Logic, defaults |

### Violations to Flag

| Violation | Severity | Example |
|-----------|----------|---------|
| API call in component | P2 | `axios.get('/api/users')` inside a component |
| Business logic in JSX | P2 | Complex conditionals in render |
| Component > 300 lines | P2 | God component — split it |
| No TypeScript interfaces for API | P3 | Using `any` for API responses |
| State management in component | P2 (if shared) | Lifting state that should be in context/hook |
| Inline styles over 3 properties | P3 | Use CSS classes |

### Correct Pattern

```typescript
// Interface
interface Transaction {
  id: number;
  amount: number;
  description: string;
  created_at: string;
}

// API Client
export const transactionApi = {
  list: () => api.get<Transaction[]>('/transactions'),
  create: (data: CreateTransactionDto) => api.post<Transaction>('/transactions', data),
};

// Hook
export function useTransactions() {
  const [transactions, setTransactions] = useState<Transaction[]>([]);
  const [loading, setLoading] = useState(true);
  // ... fetch logic
  return { transactions, loading };
}

// Component
function TransactionList() {
  const { transactions, loading } = useTransactions();
  if (loading) return <Spinner />;
  return <ul>{transactions.map(t => <TransactionItem key={t.id} transaction={t} />)}</ul>;
}
```

---

## Uniformity Check

Same feature type → same layer pattern across the codebase.

| Check | Severity |
|-------|----------|
| CRUD feature deviates from existing CRUD structure (different layering, naming, file placement) | P2 |
| New pattern introduced where an established one exists (e.g., second state-management approach, second HTTP wrapper) | P2 |
| Same concern solved two different ways in two features | P3 |

---

## Complexity Limits

| Rule | Limit | Action |
|------|-------|--------|
| Max lines per file | 500 | Split into modules |
| Max lines per function | 50 | Extract sub-functions |
| Max nesting depth | 3 | Guard clauses, early returns |
| Max function params | 4 | Use options object / DTO |
