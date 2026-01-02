# Error Handling

## Overview

Comprehensive error handling improves user experience and simplifies debugging. This document covers patterns for handling errors at all levels.

## Error Categories

| Category | Examples | Handling |
|----------|----------|----------|
| Validation | Invalid input, missing fields | Inline field errors |
| Business Logic | Duplicate entry, insufficient funds | User-friendly message |
| Network | Timeout, offline, server down | Retry option |
| Authentication | Expired session, invalid token | Redirect to login |
| Authorization | Permission denied | Access denied message |
| Server | 500 errors, unexpected response | Generic error + logging |
| Client | JavaScript errors, render failures | Error boundary |

## Error Handling Layers

```
┌─────────────────────────────────────────────────────────────┐
│ Global Error Boundary                                       │
│ └── Catches unhandled React/component errors               │
├─────────────────────────────────────────────────────────────┤
│ Route-Level Error Boundary                                  │
│ └── Catches errors within a route                          │
├─────────────────────────────────────────────────────────────┤
│ Component Try-Catch                                         │
│ └── Handles async operations                               │
├─────────────────────────────────────────────────────────────┤
│ API Client Error Handling                                   │
│ └── Transforms API errors to structured format             │
├─────────────────────────────────────────────────────────────┤
│ Form Validation                                             │
│ └── Field-level validation errors                          │
└─────────────────────────────────────────────────────────────┘
```

## Error Display Patterns

### Inline Field Errors

For form validation:

```
Email *
┌─────────────────────────────────────────────────────────────┐
│ invalid-email                                             ✕ │
└─────────────────────────────────────────────────────────────┘
⚠ Please enter a valid email address

Characteristics:
- Appears immediately below field
- Red border on input
- Icon indicating error
- Specific, actionable message
```

### Toast/Snackbar

For operation results:

```
┌─────────────────────────────────────────────────────────────┐
│ ⚠ Unable to save product. Please try again.        [✕]    │
└─────────────────────────────────────────────────────────────┘

Characteristics:
- Appears in consistent location (top-right)
- Auto-dismisses for success (3-5s)
- Stays for errors (requires dismiss)
- Brief, clear message
```

### Inline Message

For section-level errors:

```
┌─────────────────────────────────────────────────────────────┐
│ ⚠ Unable to load products                                  │
│                                                             │
│ There was a problem connecting to the server.               │
│ Please check your connection and try again.                 │
│                                                             │
│                         [Retry]                             │
└─────────────────────────────────────────────────────────────┘

Characteristics:
- Replaces content area
- Explains the problem
- Provides recovery action
```

### Error Dialog

For critical errors requiring attention:

```
┌────────────────── Error ──────────────────┐
│                                           │
│  ⚠ Session Expired                        │
│                                           │
│  Your session has expired. Please sign    │
│  in again to continue.                    │
│                                           │
│  Any unsaved changes may be lost.         │
│                                           │
│              [Sign In Again]              │
└───────────────────────────────────────────┘

Characteristics:
- Modal, blocks interaction
- Cannot be dismissed without action
- Clear next step
```

### Error Page

For route-level or fatal errors:

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│                         ⚠                                   │
│                                                             │
│                 Something went wrong                        │
│                                                             │
│   We're sorry, but something unexpected happened.           │
│   Please try refreshing the page.                           │
│                                                             │
│            [Refresh Page]  [Go to Home]                     │
│                                                             │
│   Error ID: ERR-12345 (for support)                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Error Message Guidelines

### Structure

```
What happened + Why (if known) + How to fix

Examples:
❌ "Error"
❌ "Something went wrong"
❌ "Invalid input"

✓ "Unable to save product. The product name is already in use."
✓ "Email is required."
✓ "Connection failed. Check your internet and try again."
```

### Tone

```
❌ Technical: "Error 500: Internal Server Error"
❌ Blaming: "You entered an invalid email"
❌ Alarming: "CRITICAL ERROR!"

✓ Helpful: "We couldn't save your changes. Please try again."
✓ Neutral: "Please enter a valid email address."
✓ Calm: "We're having trouble connecting. This usually fixes itself."
```

### Actionable

Always provide a next step:

```
Error: "Unable to load data"
Actions: [Retry] [Report Issue]

Error: "Session expired"
Actions: [Sign In Again]

Error: "Permission denied"
Actions: [Request Access] [Go Back]
```

## API Error Handling

### Standard Error Response

```
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Unable to save product",
    "details": [
      {
        "field": "name",
        "code": "DUPLICATE",
        "message": "A product with this name already exists"
      }
    ],
    "requestId": "req-123-abc"
  }
}
```

### Client Processing

```
async function handleSave(product) {
  try {
    await client.save(product);
    showSuccess("Product saved");
  } catch (error) {
    if (error.code === 'VALIDATION_ERROR') {
      // Map to field errors
      setFieldErrors(error.details);
    } else if (error.code === 'UNAUTHORIZED') {
      // Redirect to login
      redirectToLogin();
    } else if (error.code === 'NETWORK_ERROR') {
      // Show retry option
      showRetryDialog(error);
    } else {
      // Generic error
      showError("Unable to save product. Please try again.");
      logError(error);
    }
  }
}
```

## Error Boundaries

Catch rendering errors:

```
┌─────────────────────────────────────────────────────────────┐
│ App                                                         │
│ └── ErrorBoundary (global)                                  │
│     ├── Header                                              │
│     ├── Main                                                │
│     │   └── ErrorBoundary (route-level)                     │
│     │       └── Page Content                                │
│     │           └── ErrorBoundary (widget-level)            │
│     │               └── Complex Widget                      │
│     └── Footer                                              │
└─────────────────────────────────────────────────────────────┘

When error occurs in "Complex Widget":
1. Widget ErrorBoundary catches it
2. Shows error UI for widget only
3. Rest of page continues working
```

### Error Boundary Fallback

```
Widget Error:
┌───────────────────────────────────┐
│ ⚠ This section couldn't load     │
│                                   │
│ [Retry] [Hide]                    │
└───────────────────────────────────┘

Page Error:
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│                 ⚠ This page encountered an error           │
│                                                             │
│                 [Refresh] [Go to Home]                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Network Error Handling

### Offline Detection

```
Online → Offline:
┌─────────────────────────────────────────────────────────────┐
│ ⚠ You're offline. Changes will be saved when reconnected.  │
└─────────────────────────────────────────────────────────────┘

Offline → Online:
┌─────────────────────────────────────────────────────────────┐
│ ✓ Back online. Syncing changes...                          │
└─────────────────────────────────────────────────────────────┘
```

### Timeout Handling

```
Request timeout:
┌─────────────────────────────────────────────────────────────┐
│ ⚠ Request timed out                                        │
│                                                             │
│ The server is taking too long to respond.                   │
│                                                             │
│ [Retry]  [Cancel]                                           │
└─────────────────────────────────────────────────────────────┘
```

### Retry Logic

```
Attempt 1: Failed (network error)
  ↓ Wait 1 second
Attempt 2: Failed (network error)
  ↓ Wait 2 seconds
Attempt 3: Failed (network error)
  ↓ Show error to user

[Retry] button → Reset attempts
```

## Form Error Handling

### Validation Summary

```
┌─────────────────────────────────────────────────────────────┐
│ ⚠ Please correct the following errors:                     │
│                                                             │
│ • Name is required                                          │
│ • Email must be a valid email address                       │
│ • Price must be greater than 0                              │
└─────────────────────────────────────────────────────────────┘
```

### Field Highlighting

```
On error:
1. Scroll to first error field
2. Focus first error field
3. Show error message below field
4. Add visual indicator (red border, icon)
```

### Submit Error

```
Form submitted → Server validation fails

Response:
{
  "errors": [
    { "field": "email", "message": "Email already exists" }
  ]
}

Action:
1. Map errors to fields
2. Show field error under email input
3. Focus email field
```

## Logging and Reporting

### What to Log

```
{
  timestamp: "2024-01-15T10:30:00Z",
  errorCode: "API_ERROR",
  message: "Failed to save product",
  url: "/api/products",
  method: "POST",
  status: 500,
  userId: "user-123",
  requestId: "req-abc",
  stackTrace: "...",
  userAgent: "...",
  additionalContext: {
    productId: 456,
    action: "update"
  }
}
```

### Error Reporting

```
Automatic reporting for:
- Unhandled exceptions
- API 5xx errors
- Client-side crashes

User-initiated reporting:
[Report Issue] → Opens form with pre-filled error info
```

## Recovery Strategies

| Error Type | Recovery Strategy |
|------------|-------------------|
| Validation | Fix input, resubmit |
| Network | Retry with backoff |
| Auth expired | Re-authenticate |
| Permission | Request access |
| Not found | Navigate elsewhere |
| Server error | Retry or contact support |
| Client crash | Refresh page |

## Best Practices

1. **Never swallow errors** - Always handle or log
2. **Be specific** - Generic "error" isn't helpful
3. **Be actionable** - Provide next steps
4. **Preserve context** - Don't lose user's work
5. **Fail gracefully** - Degrade functionality, don't crash
6. **Log for debugging** - Include request IDs, stack traces
7. **Test error paths** - Error handling needs testing too
