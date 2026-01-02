# Form Components

## Overview

Forms are the primary mechanism for data input in CRUD applications. This document covers form architecture, field components, and best practices.

## Form Anatomy

```
┌─────────────────────────────────────────────────────────────────────┐
│ FORM HEADER                                                         │
│ Create Product                                          [?] Help    │
├─────────────────────────────────────────────────────────────────────┤
│ FORM BODY                                                           │
│                                                                     │
│ ┌─ Section: Basic Information ────────────────────────────────────┐ │
│ │                                                                 │ │
│ │ Label *                          Label                          │ │
│ │ ┌────────────────────────┐       ┌────────────────────────┐     │ │
│ │ │ Input                  │       │ Input                  │     │ │
│ │ └────────────────────────┘       └────────────────────────┘     │ │
│ │ Helper text                                                     │ │
│ │                                                                 │ │
│ └─────────────────────────────────────────────────────────────────┘ │
│                                                                     │
│ ▶ Advanced Options (collapsed)                                      │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│ FORM FOOTER                                                         │
│                                              [Cancel]  [Save]       │
└─────────────────────────────────────────────────────────────────────┘
```

## Field Anatomy

```
┌─────────────────────────────────────────────────────────────┐
│ Label *                              ← Label with indicator │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Input value                                  [Icon]     │ │ ← Input
│ └─────────────────────────────────────────────────────────┘ │
│ Helper text or error message                                │ ← Helper
└─────────────────────────────────────────────────────────────┘

States:
- Default: Gray border
- Focus: Primary color border + ring
- Valid: Green border + checkmark
- Invalid: Red border + error message
- Disabled: Grayed out, not interactive
- Read-only: Display value, not editable
```

## Common Field Types

### Text Input

```
Product Name *
┌─────────────────────────────────────────────────────────────┐
│ Widget Professional                                         │
└─────────────────────────────────────────────────────────────┘
Maximum 100 characters (75 remaining)

Props:
- placeholder: Hint text
- maxLength: Character limit
- prefix/suffix: $ or units
- mask: Format pattern
```

### Textarea

```
Description
┌─────────────────────────────────────────────────────────────┐
│ A professional-grade widget designed for...                 │
│                                                             │
│                                                             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
0/500 characters

Props:
- rows: Initial height
- maxLength: Character limit
- autoResize: Grow with content
```

### Number Input

```
Price *
┌───────────────────────┐
│ $           29.99     │
└───────────────────────┘

Quantity
┌───────────────────────┐
│ [-]    5        [+]   │
└───────────────────────┘

Props:
- min/max: Value limits
- step: Increment amount
- decimals: Decimal places
- prefix/suffix: Currency, units
```

### Select/Dropdown

```
Category *
┌────────────────────────────────────────────────────┬───┐
│ Electronics                                        │ ▼ │
└────────────────────────────────────────────────────┴───┘

Props:
- options: Array of choices
- placeholder: "Select..."
- clearable: Allow empty
- searchable: Filter options
- grouped: Categorized options
```

### Multi-Select

```
Tags
┌─────────────────────────────────────────────────────────────┐
│ [Featured ✕] [New ✕] [Sale ✕]                        [▼]   │
└─────────────────────────────────────────────────────────────┘

Props:
- maxItems: Selection limit
- chips: Show as removable tags
- selectAll: Allow select all
```

### Checkbox

```
□ I agree to the terms and conditions

☑ Enable notifications
  └─ Receive email updates about your account

Props:
- label: Text beside checkbox
- description: Secondary text
- indeterminate: Partial selection state
```

### Radio Group

```
Shipping Method *
○ Standard (5-7 days) - Free
● Express (2-3 days) - $9.99
○ Overnight (1 day) - $24.99

Props:
- options: Array with label, value, description
- layout: vertical | horizontal
```

### Toggle/Switch

```
Enable feature  [●═══════]  ON
Enable feature  [═══════○]  OFF

Props:
- label: Describes the setting
- onLabel/offLabel: "ON"/"OFF"
```

### Date Picker

```
Start Date *
┌────────────────────────────────────────────────────┬───┐
│ January 15, 2024                                   │ 📅│
└────────────────────────────────────────────────────┴───┘

Props:
- minDate/maxDate: Valid range
- format: Display format
- showTime: Include time picker
- range: Date range selection
```

### File Upload

```
Product Image
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│              📁 Drag file here or click to browse           │
│                                                             │
│              Accepts: JPG, PNG up to 5MB                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘

With file:
┌─────────────────────────────────────────────────────────────┐
│ ┌─────┐ product-photo.jpg                                   │
│ │ IMG │ 2.4 MB                          [Preview] [Remove]  │
│ └─────┘                                                     │
└─────────────────────────────────────────────────────────────┘

Props:
- accept: File types
- maxSize: Size limit
- multiple: Allow multiple files
- preview: Show thumbnail
```

## Form Layout

### Single Column

```
Best for: Simple forms, mobile, focused tasks

┌─────────────────────────────────┐
│ Field 1                         │
│ ┌─────────────────────────────┐ │
│ │                             │ │
│ └─────────────────────────────┘ │
│                                 │
│ Field 2                         │
│ ┌─────────────────────────────┐ │
│ │                             │ │
│ └─────────────────────────────┘ │
└─────────────────────────────────┘
```

### Two Column

```
Best for: Medium forms, related field pairs

┌─────────────────────────────────────────────────────────────┐
│ First Name                    Last Name                     │
│ ┌─────────────────────┐       ┌─────────────────────┐       │
│ │                     │       │                     │       │
│ └─────────────────────┘       └─────────────────────┘       │
│                                                             │
│ City                          State       Zip               │
│ ┌─────────────────────┐       ┌─────┐     ┌─────────┐       │
│ │                     │       │  ▼  │     │         │       │
│ └─────────────────────┘       └─────┘     └─────────┘       │
└─────────────────────────────────────────────────────────────┘
```

### Grouped Sections

```
Best for: Complex forms with logical groupings

┌─ Basic Information ───────────────────────────────────────┐
│ Name        ┌─────────────────────────────────────────┐   │
│             │                                         │   │
│             └─────────────────────────────────────────┘   │
└───────────────────────────────────────────────────────────┘

┌─ Pricing ─────────────────────────────────────────────────┐
│ Base Price  ┌─────────────┐   Sale Price  ┌─────────────┐ │
│             │             │               │             │ │
│             └─────────────┘               └─────────────┘ │
└───────────────────────────────────────────────────────────┘
```

## Validation

### Field-Level

```
Email *
┌─────────────────────────────────────────────────────────────┐
│ invalid-email                                             ✕ │
└─────────────────────────────────────────────────────────────┘
⚠ Please enter a valid email address

Rules:
- required: Field must have value
- minLength/maxLength: Character limits
- pattern: Regex validation
- custom: Business logic validation
```

### Form-Level

```
┌─────────────────────────────────────────────────────────────┐
│ ⚠ Please correct the following errors:                     │
│                                                             │
│ • Email is required                                         │
│ • Password must be at least 8 characters                    │
│ • Passwords do not match                                    │
└─────────────────────────────────────────────────────────────┘
```

### Validation Timing

| Event | Validate | Use Case |
|-------|----------|----------|
| On input | Format only | Password strength |
| On blur | Field rules | Most fields |
| On submit | All rules | Final validation |
| Async | Server check | Username availability |

## Form States

### Pristine

No changes made, submit typically disabled.

### Dirty

Changes made, prompt on navigation away.

### Submitting

```
[Saving...] ← Button shows loading state
Form disabled during submission
```

### Submitted

```
Success: Toast notification, navigate away
Error: Show error, keep form editable
```

## Form Patterns

### Create vs Edit

```
Create Mode:
- Title: "Create Product"
- Empty fields
- Submit: "Create" or "Save"

Edit Mode:
- Title: "Edit Product" or product name
- Pre-filled fields
- Submit: "Save Changes" or "Update"
- Show "Created/Modified" info
```

### View Mode

```
Display values without edit capability:
- No input borders
- Read-only styling
- Edit button to switch modes
```

### Wizard/Stepper

```
┌─────────────────────────────────────────────────────────────┐
│ ●────●────○────○                                            │
│ Step 1  Step 2  Step 3  Step 4                              │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│ Current step content                                        │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│                    [← Back]  [Next →]                       │
└─────────────────────────────────────────────────────────────┘
```

## Accessibility

| Requirement | Implementation |
|-------------|----------------|
| Labels | Every input has `<label for="">` |
| Required | `aria-required="true"` + visual * |
| Errors | `aria-describedby` links to error |
| Focus | Visible focus indicator |
| Tab order | Logical, follows visual order |
| Grouping | `<fieldset>` and `<legend>` |
