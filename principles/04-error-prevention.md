# Error Prevention

## Principle

The best error message is the one that never needs to appear. Design interfaces that prevent errors before they occur.

## Error Prevention Hierarchy

1. **Make errors impossible** - Constraints that prevent invalid input
2. **Make errors difficult** - Design that guides toward correct input
3. **Make errors recoverable** - Easy undo and correction

## Prevention Strategies

### 1. Constraints

Prevent invalid input at the interface level:

| Error Type | Prevention |
|------------|------------|
| Invalid characters | Input masks, character filters |
| Out of range | Min/max limits, sliders |
| Wrong format | Formatted inputs, pickers |
| Invalid combinations | Disable invalid options |
| Missing required | Pre-filled defaults |

#### Examples

```
Phone number:     Use input mask: (___) ___-____
Date:             Use date picker, not text input
Quantity:         Use number input with min=0
State/Province:   Use dropdown, not text input
Email:            Validate format on blur
```

### 2. Smart Defaults

Pre-fill likely values:

```
New Product Form:
  - Status: "Active" (most common)
  - Created Date: Today
  - Created By: Current user
  - Category: Last used category (if applicable)
```

#### Default Value Rules

| Scenario | Default Strategy |
|----------|------------------|
| Boolean fields | Choose the safe/common option |
| Dates | Today, or start of relevant period |
| User fields | Current user |
| Status fields | Initial state in workflow |
| Related entities | Most recently used |

### 3. Confirmation for Destructive Actions

Require explicit confirmation:

```
Low risk:    Save, Update       → No confirmation
Medium risk: Cancel with changes → "Discard changes?"
High risk:   Delete single      → "Delete this product?"
Critical:    Delete multiple    → "Delete 15 products? Type 'DELETE' to confirm"
```

#### Confirmation Dialog Guidelines

```
┌─────────── Delete Product ───────────┐
│                                      │
│ Are you sure you want to delete      │
│ "Premium Widget"?                    │
│                                      │
│ ⚠ This action cannot be undone.      │
│                                      │
│        [Cancel]  [Delete]            │
│                                      │
│ ┌──────────────────────────────────┐ │
│ │ 💡 Consider deactivating instead │ │
│ │    to preserve historical data.   │ │
│ └──────────────────────────────────┘ │
└──────────────────────────────────────┘
```

### 4. Undo Capability

Allow reversal of actions:

| Action | Undo Strategy |
|--------|---------------|
| Text edit | Ctrl+Z in input |
| Row delete | "Undo" in toast (10 seconds) |
| Bulk action | Undo option in confirmation |
| Status change | Direct reversal allowed |

#### Soft Delete Pattern

```
Instead of:  DELETE FROM products WHERE id = 123
Use:         UPDATE products SET deleted_at = NOW() WHERE id = 123

Benefits:
- Recoverable for 30 days
- Audit trail preserved
- Related data integrity maintained
```

### 5. Validation Timing

Validate at the right moment:

| When | What to Validate |
|------|------------------|
| On input | Format (as user types, debounced) |
| On blur | Required, format, simple business rules |
| On submit | All rules, server-side validation |

#### Validation Feedback

```
Empty:      [_______________]       No validation shown
Typing:     [john@exampl    ]       No validation yet
Valid:      [john@example.com] ✓    Green indicator
Invalid:    [john@]          ✗      "Enter a valid email"
                                    (shown after blur)
```

### 6. Guided Input

Help users enter correct data:

```
Autocomplete:
  [Cate________________]
   └─ Category
      Catering
      Catalog

Suggestions:
  [123-45-____]
  "Enter the last 4 digits of your SSN"

Examples:
  Email: [____________________]
  "e.g., john@example.com"
```

### 7. Disable Invalid Actions

Don't let users attempt invalid operations:

```
✅ Good: [Save] button disabled when form is invalid
✅ Good: Delete option hidden when user lacks permission
✅ Good: Submit disabled during submission

❌ Bad: Click Save → "Please fill required fields"
❌ Bad: Click Delete → "You don't have permission"
```

#### Disabled State Guidance

```
When disabled, explain why:

[Save]          ← Disabled
   ↓
   "Complete all required fields to save"
   OR
   Tooltip: "Saving... please wait"
```

### 8. Forgiving Input

Accept input variations:

```
Phone number:
  Accept: "555-123-4567", "5551234567", "(555) 123-4567"
  Store:  "5551234567" (normalized)
  Display: "(555) 123-4567" (formatted)

Date:
  Accept: "1/5/24", "01/05/2024", "Jan 5, 2024"
  Store:  "2024-01-05" (ISO format)

Search:
  Accept: "  WIDGET  ", "widget", "Widget"
  Process: "widget" (trimmed, lowercased)
```

## Error-Prone Areas in CRUD Applications

### Data Entry Forms

| Error | Prevention |
|-------|------------|
| Missed required field | Visual indicator, validate on blur |
| Duplicate entry | Check uniqueness before submit |
| Invalid format | Input masks, formatters |
| Data loss on navigation | Warn about unsaved changes |

### Bulk Operations

| Error | Prevention |
|-------|------------|
| Wrong items selected | Show count, preview affected items |
| Unintended scope | Require explicit selection |
| Irreversible action | Soft delete, undo window |

### Delete Operations

| Error | Prevention |
|-------|------------|
| Accidental delete | Confirmation dialog |
| Delete with dependencies | Show related data count |
| Mass delete | Require typing confirmation |

### Search and Filter

| Error | Prevention |
|-------|------------|
| No results confusion | Clear feedback, suggestions |
| Forgotten filters | Show active filters prominently |
| Complex filter mistakes | Visual filter builder |

## Recovery Strategies

When errors do occur:

### 1. Preserve User Input

```
❌ Bad: Form error → Clear all fields
✅ Good: Form error → Keep all input, highlight errors
```

### 2. Specific Error Messages

```
❌ "Validation failed"
✅ "Email is already registered. Sign in instead?"
```

### 3. Recovery Actions

```
Error: "Session expired"
Actions: [Sign In Again] [Save Draft Locally]

Error: "Network unavailable"
Actions: [Retry] [Work Offline]
```

### 4. Auto-Save

For long forms or complex data:

```
Auto-save draft every 30 seconds
Show: "Draft saved at 2:34 PM"
On error: "Unable to auto-save. Check connection."
```

## Checklist

- [ ] Required fields are clearly marked
- [ ] Invalid options are disabled or hidden
- [ ] Smart defaults reduce required input
- [ ] Destructive actions require confirmation
- [ ] Undo is available for reversible actions
- [ ] Validation happens at appropriate times
- [ ] Error messages explain how to fix the problem
- [ ] User input is preserved when errors occur
- [ ] Unsaved changes trigger a warning on navigation
