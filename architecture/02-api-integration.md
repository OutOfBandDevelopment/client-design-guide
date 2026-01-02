# API Integration

## Overview

Patterns for communicating with backend services in CRUD applications.

## API Client Architecture

### Layered Approach

```
┌─────────────────────────────────────────────────────────────┐
│ UI Components                                               │
│ └── Uses hooks/services, doesn't know about HTTP           │
├─────────────────────────────────────────────────────────────┤
│ Data Hooks / Services                                       │
│ └── Provides data operations, handles loading/error states │
├─────────────────────────────────────────────────────────────┤
│ API Clients                                                 │
│ └── Entity-specific operations (CRUD)                      │
├─────────────────────────────────────────────────────────────┤
│ HTTP Client (Base)                                          │
│ └── Auth, headers, error handling, interceptors            │
├─────────────────────────────────────────────────────────────┤
│ Network (fetch/axios)                                       │
└─────────────────────────────────────────────────────────────┘
```

### Base HTTP Client

```
BaseClient responsibilities:
├── Set base URL
├── Add authentication headers
├── Handle token refresh
├── Transform requests/responses
├── Global error handling
├── Request/response logging
└── Retry logic
```

### Entity Clients

```
ProductClient extends BaseClient:
├── Query(searchParams) → Promise<PagedResult<Product>>
├── Get(id) → Promise<Product>
├── Save(product) → Promise<Product>
├── Delete(id) → Promise<void>
└── BulkDelete(ids) → Promise<BulkResult>
```

## Request Patterns

### Query/List

```
GET /api/products?page=1&pageSize=25&sort=name&filter[status]=active

Request:
{
  currentPage: 0,
  pageSize: 25,
  orderBy: { name: 'asc' },
  filter: {
    status: { eq: 'active' },
    categoryId: { in: [1, 2, 3] }
  }
}

Response:
{
  rows: [...],
  totalRows: 150,
  currentPage: 0,
  pageSize: 25
}
```

### Get Single

```
GET /api/products/123

Response:
{
  productId: 123,
  name: "Widget",
  category: "Electronics",
  ...
}
```

### Create

```
POST /api/products

Request Body:
{
  name: "New Widget",
  categoryId: 1,
  price: 29.99
}

Response:
{
  productId: 456,  // Assigned by server
  name: "New Widget",
  ...
}
```

### Update

```
PUT /api/products/123

Request Body:
{
  productId: 123,
  name: "Updated Widget",
  categoryId: 2,
  price: 34.99
}

Response:
{
  productId: 123,
  name: "Updated Widget",
  ...
}
```

### Delete

```
DELETE /api/products/123

Response: 204 No Content
OR
Response: { success: true, message: "Product deleted" }
```

### Bulk Operations

```
POST /api/products/bulk-delete

Request Body:
{
  ids: [1, 2, 3, 4, 5]
}

Response:
{
  succeeded: 4,
  failed: 1,
  errors: [
    { id: 3, message: "Product is referenced by orders" }
  ]
}
```

## Filter Operators

Standard operators for query filters:

| Operator | Meaning | Example |
|----------|---------|---------|
| eq | Equals | `status: { eq: 'active' }` |
| ne | Not equals | `status: { ne: 'deleted' }` |
| gt | Greater than | `price: { gt: 100 }` |
| gte | Greater or equal | `price: { gte: 100 }` |
| lt | Less than | `price: { lt: 50 }` |
| lte | Less or equal | `price: { lte: 50 }` |
| in | In list | `categoryId: { in: [1,2,3] }` |
| notIn | Not in list | `status: { notIn: ['deleted'] }` |
| contains | String contains | `name: { contains: 'widget' }` |
| startsWith | String starts with | `sku: { startsWith: 'WGT' }` |
| between | Range | `price: { between: [10, 100] }` |
| isNull | Is null | `deletedAt: { isNull: true }` |

## Error Handling

### HTTP Status Codes

| Code | Meaning | Client Action |
|------|---------|---------------|
| 200 | Success | Process response |
| 201 | Created | Process new resource |
| 204 | No Content | Success, no body |
| 400 | Bad Request | Show validation errors |
| 401 | Unauthorized | Redirect to login |
| 403 | Forbidden | Show permission error |
| 404 | Not Found | Show not found message |
| 409 | Conflict | Show conflict resolution |
| 422 | Validation Error | Show field errors |
| 500 | Server Error | Show generic error |

### Error Response Format

```
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "email",
        "message": "Email is already registered"
      },
      {
        "field": "price",
        "message": "Price must be greater than 0"
      }
    ]
  }
}
```

### Client Error Handling

```
try {
  result = await client.Save(product);
} catch (error) {
  if (error.status === 400) {
    // Show validation errors
    setFieldErrors(error.details);
  } else if (error.status === 401) {
    // Redirect to login
    redirectToLogin();
  } else if (error.status === 409) {
    // Handle conflict
    showConflictDialog(error);
  } else {
    // Generic error
    showErrorToast("Unable to save product");
  }
}
```

## Authentication

### Token-Based Auth

```
Request:
Authorization: Bearer <access_token>

Token Refresh Flow:
1. Request fails with 401
2. Call refresh endpoint with refresh_token
3. Get new access_token
4. Retry original request
5. If refresh fails, redirect to login
```

### Token Storage

| Storage | Security | Persistence |
|---------|----------|-------------|
| Memory | High | Session only |
| sessionStorage | Medium | Session only |
| localStorage | Lower | Permanent |
| httpOnly Cookie | Highest | Configurable |

## Optimistic Updates

Update UI before server confirms:

```
1. User clicks "Save"
2. UI immediately shows success
3. Send request to server
4. If success: Done
5. If error: Revert UI, show error

Benefits:
- Instant feedback
- Better perceived performance

Risks:
- Must handle failures gracefully
- Can confuse users if reverted
```

### When to Use

```
✓ Use for:
- Toggle switches
- Like/favorite buttons
- Adding to lists
- Simple field updates

✗ Don't use for:
- Financial transactions
- Destructive actions
- Complex operations
- Data requiring validation
```

## Request Deduplication

Prevent duplicate requests:

```
Scenario: User rapidly clicks Save button

Without deduplication:
Click 1 → Request 1 (pending)
Click 2 → Request 2 (pending)  ❌ Duplicate
Click 3 → Request 3 (pending)  ❌ Duplicate

With deduplication:
Click 1 → Request 1 (pending)
Click 2 → Ignored (request in flight)
Click 3 → Ignored (request in flight)
Request 1 completes → UI updated
```

## Retry Logic

```
Retry configuration:
├── Max attempts: 3
├── Retry delay: 1s, 2s, 4s (exponential backoff)
├── Retry on: Network error, 5xx, timeout
└── Don't retry: 4xx (client errors)

Flow:
Request → Fail (5xx) → Wait 1s → Retry
                    → Fail (5xx) → Wait 2s → Retry
                                → Fail → Show error
```

## Caching Strategies

### Cache Headers

```
Cache-Control: max-age=300  // Cache for 5 minutes
Cache-Control: no-cache     // Always revalidate
Cache-Control: no-store     // Never cache
ETag: "abc123"             // For conditional requests
```

### Client-Side Cache

```
Cache Entry:
{
  key: ['products', { page: 1 }],
  data: [...],
  timestamp: 1234567890,
  ttl: 300000,  // 5 minutes
  stale: false
}

Strategies:
- stale-while-revalidate: Return cache, fetch fresh
- cache-first: Return cache if valid
- network-first: Fetch fresh, fallback to cache
```

## Pagination Patterns

### Offset Pagination

```
GET /api/products?page=2&pageSize=25

Response:
{
  rows: [...],
  totalRows: 150,
  currentPage: 2,
  totalPages: 6
}

Pros: Simple, supports jump to page
Cons: Inconsistent with concurrent writes
```

### Cursor Pagination

```
GET /api/products?cursor=abc123&limit=25

Response:
{
  rows: [...],
  nextCursor: "def456",
  hasMore: true
}

Pros: Consistent results, better performance
Cons: Can't jump to page, can't show total
```

## WebSocket Integration

For real-time updates:

```
Use Cases:
- Live notifications
- Real-time data updates
- Collaborative editing
- Chat/messaging

Pattern:
1. Initial data via REST
2. Subscribe to updates via WebSocket
3. Apply incremental updates
4. Reconnect logic for disconnections
```

## Best Practices

1. **Use typed clients** - Generate from OpenAPI/Swagger
2. **Centralize error handling** - In base client
3. **Handle all states** - Loading, error, empty, success
4. **Log requests** - For debugging (not in production)
5. **Set timeouts** - Don't wait forever
6. **Cancel on unmount** - Prevent state updates on unmounted components
7. **Debounce user input** - Reduce request volume
8. **Show loading states** - Always indicate activity
