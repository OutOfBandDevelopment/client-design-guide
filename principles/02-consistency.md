# Consistency

## Principle

Similar things should look and behave similarly. Users build mental models based on patterns—consistency reinforces those patterns and reduces learning time.

## Types of Consistency

### 1. Visual Consistency

Same visual treatment for same concepts:

| Element | Must Be Consistent |
|---------|-------------------|
| Colors | Status colors, action colors, brand colors |
| Typography | Headings, body, labels, links |
| Spacing | Margins, padding, gaps |
| Icons | Same icon = same meaning everywhere |
| Buttons | Primary, secondary, danger styling |

#### Example: Status Colors

```
Define once, use everywhere:

Success/Active:    Green  (#22c55e)
Warning/Pending:   Yellow (#f59e0b)
Error/Inactive:    Red    (#ef4444)
Info/Neutral:      Blue   (#3b82f6)
```

### 2. Behavioral Consistency

Same interactions produce same results:

| Action | Expected Behavior |
|--------|-------------------|
| Click row | Select row OR navigate to detail (pick one) |
| Double-click | Open for editing |
| Right-click | Context menu |
| Drag | Reorder or move |
| Enter key | Submit form |
| Escape key | Cancel/close |

#### Example: Selection Behavior

```
If clicking a row SELECTS it in the Products grid,
clicking a row must SELECT it in the Categories grid.

NOT: Select in Products, Navigate in Categories
```

### 3. Functional Consistency

Same features work the same way:

| Feature | Consistent Implementation |
|---------|--------------------------|
| Search | Same placement, same behavior |
| Filtering | Same filter UI across all grids |
| Sorting | Same click behavior, same indicators |
| Pagination | Same controls, same page sizes |
| Export | Same formats, same button placement |

### 4. Linguistic Consistency

Same words for same concepts:

```
❌ Inconsistent:
   - "Delete" vs "Remove" vs "Trash"
   - "Save" vs "Submit" vs "Update"
   - "Cancel" vs "Close" vs "Dismiss"

✅ Consistent:
   - "Delete" for removing records
   - "Save" for persisting changes
   - "Cancel" for abandoning changes
```

## Consistency Hierarchy

When consistency conflicts, prioritize in this order:

1. **Platform conventions** (OS, browser standards)
2. **Industry conventions** (CRUD app patterns)
3. **Product conventions** (your app's patterns)
4. **Page conventions** (within-page patterns)

### Example: Platform vs Product

```
Platform: Enter submits forms
Product:  You want Ctrl+S to save

Resolution: Support BOTH
- Enter submits
- Ctrl+S also saves
```

## Building Consistent Systems

### Design Tokens

Define reusable values:

```
// Spacing scale
--spacing-xs: 4px
--spacing-sm: 8px
--spacing-md: 16px
--spacing-lg: 24px
--spacing-xl: 32px

// Color tokens
--color-primary: #3b82f6
--color-success: #22c55e
--color-danger: #ef4444

// Typography
--font-size-sm: 12px
--font-size-md: 14px
--font-size-lg: 16px
```

### Component Library

Create reusable components with consistent APIs:

```
Button:
  - variant: 'primary' | 'secondary' | 'danger'
  - size: 'small' | 'medium' | 'large'
  - disabled: boolean
  - loading: boolean

// Every button in the app uses these same props
```

### Pattern Library

Document recurring patterns:

```
Data Grid Pattern:
  - Title bar with actions
  - Filter sidebar (collapsible)
  - Column headers with sort indicators
  - Rows with selection checkboxes
  - Pagination footer
  - Export button top-right
```

## Consistency Checklist by Area

### Forms

- [ ] Labels positioned consistently (top or left)
- [ ] Required field indicators consistent
- [ ] Validation messages same style/position
- [ ] Button order consistent (Primary right or left)
- [ ] Field widths follow a system

### Data Grids

- [ ] Column alignment consistent by data type
- [ ] Action buttons same position in rows
- [ ] Selection behavior identical across grids
- [ ] Empty states styled the same
- [ ] Loading states styled the same

### Navigation

- [ ] Menu structure follows same pattern
- [ ] Breadcrumbs always present (or never)
- [ ] Active state indicators consistent
- [ ] Icon usage consistent

### Dialogs/Modals

- [ ] Same max-width rules
- [ ] Close button same position
- [ ] Button order in footer consistent
- [ ] Backdrop behavior consistent

### Notifications

- [ ] Same positioning (top-right, bottom, etc.)
- [ ] Same duration rules
- [ ] Same dismissal behavior
- [ ] Same severity styling

## Breaking Consistency Intentionally

Sometimes inconsistency is correct:

### When to Break Consistency

1. **Destructive actions** - Make delete look different (red, warning icon)
2. **Critical alerts** - Should stand out from normal notifications
3. **First-time experiences** - Onboarding can break patterns to teach

### How to Break Consistency

```
Breaking consistency for Delete:

Standard button:    [Save]     Blue, solid
Danger button:      [Delete]   Red, outlined, requires confirmation

The inconsistency is intentional and meaningful.
```

## Measuring Consistency

### Audit Checklist

Periodically review:

- [ ] Screenshot every form—do they look the same?
- [ ] List all button labels—are synonyms eliminated?
- [ ] Compare all data grids—same features, same behavior?
- [ ] Check all error messages—same format?
- [ ] Review all icons—same icon, same meaning?

### Consistency Score

Rate each area 1-5:

| Area | Score | Notes |
|------|-------|-------|
| Visual styling | 4 | Some old pages use old colors |
| Interaction patterns | 5 | All grids work identically |
| Terminology | 3 | "Remove" and "Delete" both used |
| Navigation | 5 | Consistent sidebar everywhere |

## Anti-Patterns

### "Just This Once"

```
❌ "For this page, let's put the filter on the right"
❌ "Here we'll use a modal, but elsewhere use inline"
❌ "This button should be orange because it's special"
```

### Inconsistency Debt

Every inconsistency:
- Confuses users
- Increases maintenance cost
- Makes future changes harder
- Requires documentation

### The Fix

When you find inconsistency:
1. Document the standard
2. Update the outliers
3. Prevent future inconsistency (component library, code review)
