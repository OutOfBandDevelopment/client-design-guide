# Feedback and Response

## Principle

Every user action must produce visible, immediate feedback. Users need to know that the system received their input and what happened as a result.

## Response Time Guidelines

| Duration | User Perception | Design Response |
|----------|-----------------|-----------------|
| 0-100ms | Instantaneous | Direct manipulation (hover, focus) |
| 100-300ms | Slight delay | Show action acknowledged (button press) |
| 300ms-1s | Noticeable | Show progress indicator |
| 1-10s | Waiting | Show spinner with message |
| >10s | Long wait | Show progress bar with percentage |

## Types of Feedback

### 1. Immediate Feedback (0-100ms)

Visual response to user input:

```
Action              → Immediate Feedback
──────────────────────────────────────────
Mouse hover         → Highlight, cursor change
Button press        → Depress effect, color change
Focus               → Ring/outline
Checkbox click      → Check appears
Text input          → Characters appear
Drag start          → Element lifts, shadow
```

#### Implementation

```css
/* Button press feedback */
.button:active {
  transform: scale(0.98);
  transition: transform 50ms;
}

/* Focus feedback */
.input:focus {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

### 2. Progress Feedback (300ms+)

For operations that take time:

| Duration | Feedback Type |
|----------|---------------|
| 300ms-2s | Spinner (indeterminate) |
| 2s-10s | Spinner with message |
| >10s | Progress bar with percentage |
| Very long | Progress + time estimate |

#### Spinner Patterns

```
Loading data:    [Spinner] Loading products...
Saving:          [Spinner] Saving changes...
Processing:      [====>    ] 45% Processing 450 of 1000 items
```

#### Skeleton Loading

For content-heavy pages, show content structure while loading:

```
┌─────────────────────────────────────┐
│ ████████████                        │  ← Title placeholder
├─────────────────────────────────────┤
│ ████████  │ ██████  │ ████████████  │  ← Row placeholders
│ ██████    │ ████████│ ██████        │
│ ████████  │ ██████  │ ████          │
└─────────────────────────────────────┘
```

### 3. Completion Feedback

Confirm action completed:

| Action | Feedback |
|--------|----------|
| Save successful | Toast: "Product saved successfully" |
| Delete completed | Toast + item removed from list |
| Export ready | Toast with download link |
| Bulk action done | Toast: "Updated 15 products" |

#### Toast Message Guidelines

```
✅ Good:
   "Product 'Widget X' saved successfully"
   "15 products updated"
   "Export ready - Download"

❌ Bad:
   "Success"              (too vague)
   "Operation completed"  (too vague)
   "Done"                 (uninformative)
```

### 4. Error Feedback

Communicate problems clearly:

```
┌──────────────────────────────────────────┐
│ ⚠ Error                                  │
│                                          │
│ Unable to save product.                  │
│ The product name is already in use.      │
│                                          │
│ [Try Again]  [Edit Name]                 │
└──────────────────────────────────────────┘
```

#### Error Message Formula

```
What happened + Why it happened + How to fix it

Example:
"Unable to delete category.
 It contains 15 products.
 Remove or reassign products first."
```

### 5. Validation Feedback

Real-time input validation:

| Timing | Use Case |
|--------|----------|
| On blur | Most fields |
| On input | Passwords, usernames (debounced) |
| On submit | Final validation pass |

#### Validation States

```
Empty:      [___________________]  No indicator
Valid:      [john@example.com ✓]  Green checkmark
Invalid:    [john@            ✗]  Red with message
            "Please enter a valid email address"
Validating: [john@example.com ⟳]  Spinner while checking
```

## Feedback Placement

### Inline Feedback

Position feedback near the triggering element:

```
Form field:
┌────────────────────────────────┐
│ Email                          │
│ ┌──────────────────────────┐   │
│ │ john@                    │   │
│ └──────────────────────────┘   │
│ ⚠ Please enter a valid email   │  ← Directly below field
└────────────────────────────────┘
```

### Global Feedback

For actions affecting the whole page:

```
┌─ Toast (top-right) ─────────────┐
│ ✓ Product saved successfully    │
└─────────────────────────────────┘

┌─────────────────────────────────┐
│  Page Content                   │
│                                 │
└─────────────────────────────────┘
```

### Modal Feedback

For important confirmations or errors:

```
┌──────────── Confirm ────────────┐
│                                 │
│  Delete 5 selected products?    │
│                                 │
│  This action cannot be undone.  │
│                                 │
│        [Cancel]  [Delete]       │
└─────────────────────────────────┘
```

## Feedback Persistence

| Feedback Type | Duration |
|---------------|----------|
| Success toast | 3-5 seconds, auto-dismiss |
| Error toast | Manual dismiss or 10+ seconds |
| Validation error | Until corrected |
| Modal | Until user action |
| Inline message | Until condition changes |

## Optimistic Updates

Update UI before server confirms (for responsive feel):

```
User clicks "Save"
  → UI immediately shows "Saved" ✓
  → Send request to server
  → If error: Revert UI, show error message
```

### When to Use Optimistic Updates

```
✅ Use for:
   - Toggle switches
   - Adding to lists
   - Simple edits
   - Like/favorite actions

❌ Don't use for:
   - Financial transactions
   - Destructive actions
   - Complex operations
   - Actions requiring validation
```

## Feedback for Different States

### Empty States

When there's no data:

```
┌─────────────────────────────────┐
│                                 │
│          📦                     │
│    No products found            │
│                                 │
│    Get started by creating      │
│    your first product.          │
│                                 │
│    [+ Create Product]           │
│                                 │
└─────────────────────────────────┘
```

### Zero Results

When search/filter returns nothing:

```
┌─────────────────────────────────┐
│                                 │
│          🔍                     │
│    No results for "xyz"         │
│                                 │
│    • Check your spelling        │
│    • Try different keywords     │
│    • Clear filters              │
│                                 │
│    [Clear Filters]              │
│                                 │
└─────────────────────────────────┘
```

### Error States

When something goes wrong:

```
┌─────────────────────────────────┐
│                                 │
│          ⚠️                      │
│    Unable to load products      │
│                                 │
│    There was a problem          │
│    connecting to the server.    │
│                                 │
│    [Retry]  [Report Issue]      │
│                                 │
└─────────────────────────────────┘
```

## Anti-Patterns

### Silent Failures

```
❌ User clicks Save → Nothing happens
❌ Request fails → No error shown
❌ Validation fails → Form just doesn't submit
```

### Feedback Overload

```
❌ Toast for every minor action
❌ Multiple simultaneous modals
❌ Sounds + animations + toasts together
```

### Vague Messages

```
❌ "Error"
❌ "Something went wrong"
❌ "Operation failed"
❌ "Invalid input"
```

## Checklist

- [ ] Every click produces visible feedback within 100ms
- [ ] Operations >300ms show loading indicator
- [ ] Success messages are specific and helpful
- [ ] Error messages explain what happened and how to fix
- [ ] Validation feedback appears inline, near the field
- [ ] Empty states guide users to next action
- [ ] Loading states don't block the entire UI unnecessarily
