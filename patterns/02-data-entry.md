# Data Entry Patterns

## Overview

Patterns for collecting and validating user input in CRUD applications.

## Form Layout Patterns

### Single Column Form

Best for simple forms (< 10 fields).

```
┌─────────────────────────────────────────┐
│ Create Product                          │
├─────────────────────────────────────────┤
│ Product Name *                          │
│ ┌─────────────────────────────────────┐ │
│ │                                     │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ Category *                              │
│ ┌─────────────────────────────────┐     │
│ │ Select category...           ▼  │     │
│ └─────────────────────────────────┘     │
│                                         │
│ Description                             │
│ ┌─────────────────────────────────────┐ │
│ │                                     │ │
│ │                                     │ │
│ │                                     │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ Price *                                 │
│ ┌───────────┐                           │
│ │ $         │                           │
│ └───────────┘                           │
│                                         │
│           [Cancel]  [Save Product]      │
└─────────────────────────────────────────┘
```

### Two Column Form

For medium complexity (10-20 fields).

```
┌─────────────────────────────────────────────────────────────┐
│ Create Product                                              │
├─────────────────────────────────────────────────────────────┤
│ Product Name *                   SKU                        │
│ ┌──────────────────────┐         ┌──────────────────────┐   │
│ │                      │         │                      │   │
│ └──────────────────────┘         └──────────────────────┘   │
│                                                             │
│ Category *                       Subcategory                │
│ ┌──────────────────────┐         ┌──────────────────────┐   │
│ │ Select...         ▼  │         │ Select...         ▼  │   │
│ └──────────────────────┘         └──────────────────────┘   │
│                                                             │
│ Price *                          Quantity                   │
│ ┌──────────────────────┐         ┌──────────────────────┐   │
│ │ $                    │         │                      │   │
│ └──────────────────────┘         └──────────────────────┘   │
│                                                             │
│                              [Cancel]  [Save Product]       │
└─────────────────────────────────────────────────────────────┘
```

### Grouped/Sectioned Form

For complex forms with logical groupings.

```
┌─────────────────────────────────────────────────────────────┐
│ Create Product                                              │
├─────────────────────────────────────────────────────────────┤
│ ┌─ Basic Information ─────────────────────────────────────┐ │
│ │ Product Name *             SKU                          │ │
│ │ ┌───────────────┐          ┌───────────────┐            │ │
│ │ │               │          │               │            │ │
│ │ └───────────────┘          └───────────────┘            │ │
│ └─────────────────────────────────────────────────────────┘ │
│                                                             │
│ ┌─ Pricing ───────────────────────────────────────────────┐ │
│ │ Base Price *               Sale Price                   │ │
│ │ ┌───────────────┐          ┌───────────────┐            │ │
│ │ │ $             │          │ $             │            │ │
│ │ └───────────────┘          └───────────────┘            │ │
│ │                                                         │ │
│ │ □ Apply bulk discount                                   │ │
│ └─────────────────────────────────────────────────────────┘ │
│                                                             │
│ ▶ Advanced Settings                                         │
│                                                             │
│                              [Cancel]  [Save Product]       │
└─────────────────────────────────────────────────────────────┘
```

### Wizard/Stepper Form

For complex, sequential data entry.

```
┌─────────────────────────────────────────────────────────────┐
│ Create Product                                              │
├─────────────────────────────────────────────────────────────┤
│  ●──────●──────○──────○                                     │
│ Basic  Pricing Inventory Review                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│ Step 2: Pricing                                             │
│                                                             │
│ Base Price *                                                │
│ ┌─────────────────────────────────────┐                     │
│ │ $                                   │                     │
│ └─────────────────────────────────────┘                     │
│                                                             │
│ Cost Price                                                  │
│ ┌─────────────────────────────────────┐                     │
│ │ $                                   │                     │
│ └─────────────────────────────────────┘                     │
│                                                             │
│             [← Back]                    [Next: Inventory →] │
└─────────────────────────────────────────────────────────────┘
```

## Field Types

### Text Input

```
Label *
┌─────────────────────────────────┐
│ Placeholder text...             │
└─────────────────────────────────┘
Helper text or character count
```

### Dropdown/Select

```
Category *
┌─────────────────────────────────┬───┐
│ Select category...              │ ▼ │
└─────────────────────────────────┴───┘

When open:
┌─────────────────────────────────┬───┐
│ Electronics                     │ ▲ │
├─────────────────────────────────┴───┤
│   Electronics                       │ ← Selected
│   Clothing                          │
│   Home & Garden                     │
│   Sports & Outdoors                 │
└─────────────────────────────────────┘
```

### Multi-Select

```
Categories *
┌─────────────────────────────────────┐
│ [Electronics ×] [Clothing ×]  [+2]  │
└─────────────────────────────────────┘
```

### Date Picker

```
Start Date *
┌─────────────────────────────┬───┐
│ 01/15/2024                  │ 📅│
└─────────────────────────────┴───┘

When open:
┌──────────────────────────────────┐
│       ◄  January 2024  ►         │
├──────────────────────────────────┤
│ Su  Mo  Tu  We  Th  Fr  Sa       │
│     1   2   3   4   5   6        │
│ 7   8   9  10  11  12  13        │
│ 14 [15] 16  17  18  19  20       │ ← Selected
│ 21  22  23  24  25  26  27       │
│ 28  29  30  31                   │
└──────────────────────────────────┘
```

### Boolean/Toggle

```
Option A: Checkbox
□ Enable notifications

Option B: Toggle Switch
Send notifications  [====○]  OFF
Send notifications  [●====]  ON

Option C: Radio Group
Status
○ Active
● Inactive
○ Pending
```

### File Upload

```
Product Image
┌─────────────────────────────────────┐
│                                     │
│     📁 Drag files here or click     │
│        to browse                    │
│                                     │
│     Accepts: JPG, PNG (max 5MB)     │
│                                     │
└─────────────────────────────────────┘

With file:
┌─────────────────────────────────────┐
│ ┌─────┐                             │
│ │ IMG │ product-photo.jpg           │
│ └─────┘ 2.4 MB           [Remove]   │
└─────────────────────────────────────┘
```

## Validation Patterns

### Validation Timing

| When | What | Example |
|------|------|---------|
| On input | Format feedback | Password strength |
| On blur | Field validation | Email format |
| On submit | Full validation | All rules checked |

### Validation States

```
Empty (untouched):
┌─────────────────────────────────┐
│                                 │
└─────────────────────────────────┘

Valid:
┌─────────────────────────────────┐
│ john@example.com              ✓ │
└─────────────────────────────────┘

Invalid:
┌─────────────────────────────────┐
│ john@                         ✗ │
└─────────────────────────────────┘
⚠ Please enter a valid email address

Processing:
┌─────────────────────────────────┐
│ john@example.com              ⟳ │
└─────────────────────────────────┘
Checking availability...
```

### Error Message Placement

```
✓ Best: Directly below field
  Email
  ┌──────────────────────┐
  │ invalid              │
  └──────────────────────┘
  ⚠ Please enter a valid email

✗ Avoid: Summary at top only
  ┌──────────────────────────────┐
  │ ⚠ Please fix 3 errors below │
  └──────────────────────────────┘
  (user must hunt for errors)
```

### Inline vs Summary

```
Best: Both inline AND summary

┌──────────────────────────────────────┐
│ ⚠ Please correct 2 errors below     │
└──────────────────────────────────────┘

Email *
┌──────────────────────────────────────┐
│ invalid                              │
└──────────────────────────────────────┘
⚠ Please enter a valid email

Password *
┌──────────────────────────────────────┐
│ ••••                                 │
└──────────────────────────────────────┘
⚠ Password must be at least 8 characters
```

## Form Behavior

### Auto-Save

For complex forms or long processes:

```
┌─────────────────────────────────────────┐
│ Edit Article                            │
│                            Saved ✓ 2:34 │
├─────────────────────────────────────────┤
│ ...                                     │
```

### Unsaved Changes Warning

```
┌────────────────── Unsaved Changes ──────────────────┐
│                                                     │
│ You have unsaved changes. What would you like to do?│
│                                                     │
│    [Discard Changes]  [Save and Leave]  [Stay]      │
└─────────────────────────────────────────────────────┘
```

### Form Submission States

```
Idle:       [Save Product]
Submitting: [Saving...   ] (disabled, spinner)
Success:    Toast: "Product saved successfully"
Error:      [Save Product] + Error message
```

## Dependent Fields

### Cascading Dropdowns

```
Country                     State
┌──────────────────┐        ┌──────────────────┐
│ United States  ▼ │   →    │ Select state...▼ │
└──────────────────┘        └──────────────────┘
                            (enabled after country selected)
```

### Conditional Fields

```
Shipping Type
○ Standard
● Express
○ Pickup

[If Express selected:]
┌─ Express Options ─────────────────────┐
│ Delivery Date *                       │
│ ┌────────────────────────┐            │
│ │ Select date...       📅│            │
│ └────────────────────────┘            │
│                                       │
│ □ Require signature                   │
└───────────────────────────────────────┘
```

## Accessibility Requirements

- All inputs have associated labels
- Required fields marked with * and aria-required
- Error messages linked to inputs via aria-describedby
- Focus moves to first error on failed submission
- Tab order follows visual order
- Color is not sole indicator of state

## Button Placement

### Consistent Button Order

```
Option A: Primary on right (recommended for LTR)
          [Cancel]  [Save]

Option B: Primary on left
          [Save]  [Cancel]

Pick one and use consistently throughout app.
```

### Button States

```
Form incomplete:  [Save] (disabled)
Form valid:       [Save] (enabled)
Submitting:       [Saving...] (disabled, loading)
After success:    Navigate away OR [Save] (enabled again)
```
