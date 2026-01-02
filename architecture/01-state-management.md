# State Management

## Overview

Effective state management is critical for maintainable client applications. This document covers patterns for managing different types of state.

## State Categories

| Category | Scope | Persistence | Examples |
|----------|-------|-------------|----------|
| UI State | Component | None | Open/closed, hover, focus |
| Form State | Form | Session | Field values, validation |
| Application State | Global | Session | User, theme, notifications |
| Server State | Global | Cache | API data, entities |
| URL State | Global | URL | Filters, pagination, tabs |

## UI State (Local)

State that exists only within a component.

### Examples

- Modal open/closed
- Dropdown expanded
- Tab selection
- Input focus
- Hover state
- Loading spinner

### Pattern

```
Component:
┌─────────────────────────────────────┐
│ [isOpen]     ← Local state          │
│ [isLoading]  ← Local state          │
│                                     │
│ onClick → setIsOpen(true)           │
│ onClose → setIsOpen(false)          │
└─────────────────────────────────────┘

No external dependencies
No persistence needed
Changes don't affect other components
```

### Best Practices

- Keep state closest to where it's used
- Don't lift state unless needed by siblings/parent
- Reset on unmount (default behavior)

## Form State

State for data entry forms.

### What to Track

```
Form State:
├─ values: { name: "Widget", price: 29.99 }
├─ touched: { name: true, price: false }
├─ errors: { name: null, price: "Required" }
├─ dirty: true (has unsaved changes)
├─ valid: false
└─ submitting: false
```

### Patterns

#### Controlled Forms

Every input value in state, updated on change.

```
Pros:
+ Full control over values
+ Real-time validation
+ Easy programmatic updates

Cons:
- More re-renders
- More boilerplate
```

#### Uncontrolled Forms

Values stored in DOM, read on submit.

```
Pros:
+ Better performance
+ Less code

Cons:
- Less control
- Harder validation
```

#### Form Libraries

Use a form library for complex forms:

```
Benefits:
- Handles touched/dirty/validation state
- Reduces boilerplate
- Consistent patterns
- Built-in validation

Examples:
- React Hook Form
- Formik
- Vue Formulate
- Angular Reactive Forms
```

## Application State (Global)

State shared across multiple components.

### Examples

- Current user / authentication
- Theme preferences
- Notification queue
- Feature flags
- Shopping cart

### Patterns

#### Context Pattern

For read-heavy, infrequently changing data.

```
Context Provider (top of tree):
┌─────────────────────────────────────┐
│ AuthContext.Provider                │
│ ├─ value: { user, login, logout }   │
│ └─ Children consume via useContext  │
└─────────────────────────────────────┘

Best for:
- User/auth state
- Theme
- Localization
- Feature flags
```

#### Store Pattern

For complex state with many actions.

```
Store:
┌─────────────────────────────────────┐
│ State                               │
│ ├─ entities: { products: {...} }    │
│ ├─ ui: { sidebar: 'open' }          │
│ └─ session: { user: {...} }         │
│                                     │
│ Actions                             │
│ ├─ LOAD_PRODUCTS                    │
│ ├─ ADD_TO_CART                      │
│ └─ UPDATE_USER                      │
│                                     │
│ Selectors                           │
│ ├─ selectProducts()                 │
│ ├─ selectCartTotal()                │
│ └─ selectCurrentUser()              │
└─────────────────────────────────────┘

Best for:
- E-commerce cart
- Complex UI state
- Multi-step workflows
- Undo/redo functionality
```

## Server State

Data from API that needs caching and synchronization.

### Challenges

- Caching
- Background updates
- Stale data
- Optimistic updates
- Error handling
- Loading states

### Pattern: Server State Libraries

```
Query Cache:
┌─────────────────────────────────────┐
│ Cache Key: ['products', { page: 1 }]│
│ ├─ data: [...products]              │
│ ├─ status: 'success'                │
│ ├─ fetchedAt: timestamp             │
│ ├─ isStale: false                   │
│ └─ isFetching: false                │
└─────────────────────────────────────┘

Features:
- Automatic caching
- Background refetch
- Stale-while-revalidate
- Pagination support
- Optimistic updates
- Request deduplication

Examples:
- TanStack Query (React Query)
- SWR
- Apollo Client
- RTK Query
```

### Cache Strategies

| Strategy | Behavior | Use Case |
|----------|----------|----------|
| Cache-first | Return cache, fetch in background | Lists, reference data |
| Network-first | Fetch fresh, fallback to cache | Critical data |
| Stale-while-revalidate | Return stale, update when fresh arrives | Most common |
| Cache-only | Never fetch | Offline mode |
| Network-only | Never cache | Real-time data |

## URL State

State stored in the URL for shareability and navigation.

### What to Store in URL

```
✓ Store in URL:
- Current page/view
- Pagination (page, pageSize)
- Filters
- Sort order
- Selected tab
- Search query
- Entity ID being viewed

✗ Don't store in URL:
- Form field values (use form state)
- UI state (modal open)
- Temporary selections
- Sensitive data
```

### URL Patterns

```
List with filters:
/products?page=2&sort=name&category=electronics&status=active

Detail view:
/products/123

Edit view:
/products/123/edit

Tab selection:
/products/123?tab=inventory
```

### Synchronization

```
URL ↔ State synchronization:

1. On mount: Read URL → Initialize state
2. On state change: Update URL (replace, not push)
3. On URL change (back/forward): Update state
```

## State Hierarchy

```
                    ┌─────────────┐
                    │   URL       │  ← Shareable, bookmarkable
                    └──────┬──────┘
                           │
              ┌────────────┴────────────┐
              │                         │
       ┌──────┴──────┐          ┌──────┴──────┐
       │ Application │          │   Server    │  ← Global, cached
       │   State     │          │   State     │
       └──────┬──────┘          └──────┬──────┘
              │                         │
       ┌──────┴──────────────────────────┴──────┐
       │              Component Tree            │
       │                                        │
       │  ┌──────────┐    ┌──────────┐          │
       │  │ Form     │    │ UI       │          │  ← Local
       │  │ State    │    │ State    │          │
       │  └──────────┘    └──────────┘          │
       └────────────────────────────────────────┘
```

## Best Practices

### 1. Single Source of Truth

Each piece of state should exist in exactly one place.

```
❌ Bad: Same data in multiple places
Component A state: { user: {...} }
Component B state: { user: {...} }  // Duplicate!

✓ Good: Single source, derived where needed
Context: { user: {...} }
Component A: uses context
Component B: uses context
```

### 2. Derive Don't Store

Calculate values instead of storing them.

```
❌ Bad: Store calculated value
state = {
  items: [...],
  itemCount: 5,  // Redundant!
  totalPrice: 100  // Redundant!
}

✓ Good: Derive from source data
state = { items: [...] }
itemCount = items.length
totalPrice = items.reduce(...)
```

### 3. Normalize Complex Data

Store relational data in normalized form.

```
❌ Bad: Nested/duplicated
{
  orders: [
    { id: 1, customer: { id: 1, name: "John" } },
    { id: 2, customer: { id: 1, name: "John" } }  // Duplicate!
  ]
}

✓ Good: Normalized
{
  orders: { 1: { customerId: 1 }, 2: { customerId: 1 } },
  customers: { 1: { name: "John" } }
}
```

### 4. Colocation

Keep state close to where it's used.

```
If state is used by:
- One component → Local state
- Sibling components → Lift to parent
- Distant components → Context or store
- Needs persistence → URL or storage
```

## Anti-Patterns

```
❌ Prop drilling through many levels
❌ Global state for everything
❌ Duplicating server state locally
❌ Storing derived data
❌ Mutating state directly
❌ Mixing UI state with domain state
❌ Not handling loading/error states
```
