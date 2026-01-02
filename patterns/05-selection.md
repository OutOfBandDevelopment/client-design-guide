# Selection Patterns

## Overview

Patterns for selecting one or more items for viewing, editing, or batch operations.

## Selection Types

### Single Selection

Select one item at a time.

```
┌────────────────────────────────────────────────────────┐
│ Name              │ Category    │ Price   │ Actions   │
├───────────────────┼─────────────┼─────────┼───────────┤
│ Widget Pro        │ Electronics │ $29.99  │ ⋯         │ ← Selected
│ Gadget Basic      │ Electronics │ $19.99  │ ⋯         │
│ Tool Master       │ Hardware    │ $49.99  │ ⋯         │
└────────────────────────────────────────────────────────┘

Selection indicated by:
- Background highlight
- Left border accent
- Row checkbox (if present)
```

### Multi-Selection

Select multiple items for batch operations.

```
┌────────────────────────────────────────────────────────┐
│ □ │ Name              │ Category    │ Price   │ Actions│
├───┼───────────────────┼─────────────┼─────────┼────────┤
│ ☑ │ Widget Pro        │ Electronics │ $29.99  │ ⋯      │
│ ☑ │ Gadget Basic      │ Electronics │ $19.99  │ ⋯      │
│ □ │ Tool Master       │ Hardware    │ $49.99  │ ⋯      │
└────────────────────────────────────────────────────────┘

Header checkbox:
☐ = None selected
☑ = All selected
▣ = Some selected (indeterminate)
```

### Range Selection

Select contiguous items.

```
Click first item
Shift+Click last item
→ All items between are selected

│ ☑ │ Item 1            │  ← First click
│ ☑ │ Item 2            │  ← Auto-selected
│ ☑ │ Item 3            │  ← Auto-selected
│ ☑ │ Item 4            │  ← Shift+click
│ □ │ Item 5            │
```

### Toggle Selection

Add/remove from selection individually.

```
Ctrl+Click (or Cmd+Click on Mac)
→ Toggle single item without affecting others

│ ☑ │ Item 1            │  ← Previously selected
│ □ │ Item 2            │
│ ☑ │ Item 3            │  ← Ctrl+click to add
│ □ │ Item 4            │
```

## Selection UI Components

### Checkbox Column

Standard for data grids.

```
┌───┬───────────────────────────────────────────────────┐
│ □ │ Header row has select-all checkbox               │
├───┼───────────────────────────────────────────────────┤
│ ☑ │ Selected row                                     │
│ □ │ Unselected row                                   │
│ ☑ │ Selected row                                     │
└───┴───────────────────────────────────────────────────┘
```

### Radio Buttons

For single selection from a list.

```
Select a product:
○ Widget Pro      $29.99
● Gadget Basic    $19.99   ← Selected
○ Tool Master     $49.99
```

### Highlight Selection

Row highlighting without checkboxes (for view/navigate selection).

```
┌───────────────────────────────────────────────────────┐
│ Widget Pro        │ Electronics │ $29.99  │           │
├───────────────────────────────────────────────────────┤
│ Gadget Basic      │ Electronics │ $19.99  │           │ ← Highlighted
├───────────────────────────────────────────────────────┤
│ Tool Master       │ Hardware    │ $49.99  │           │
└───────────────────────────────────────────────────────┘
```

### Card Selection

For card-based layouts.

```
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│ ☑           │  │             │  │ ☑           │
│  [Image]    │  │  [Image]    │  │  [Image]    │
│             │  │             │  │             │
│ Widget Pro  │  │ Gadget      │  │ Tool Master │
│ $29.99      │  │ $19.99      │  │ $49.99      │
└─────────────┘  └─────────────┘  └─────────────┘
    Selected        Normal           Selected

Selection indicators:
- Check badge in corner
- Border highlight
- Background color change
```

## Selection Feedback

### Selection Count

Show how many items are selected.

```
┌─────────────────────────────────────────────────────────────┐
│ Products                              3 of 150 selected     │
├─────────────────────────────────────────────────────────────┤
│                     Data Grid                               │
└─────────────────────────────────────────────────────────────┘
```

### Selection Actions Bar

Show available actions for selection.

```
No selection:
┌─────────────────────────────────────────────────────────────┐
│ Products                                       [+ Create]   │
└─────────────────────────────────────────────────────────────┘

With selection:
┌─────────────────────────────────────────────────────────────┐
│ 3 selected    [Delete]  [Export]  [Assign Category]  [✕]   │
└─────────────────────────────────────────────────────────────┘
```

### Selection Persistence

| Scenario | Behavior |
|----------|----------|
| Page navigation | Clear selection (usually) |
| Filter change | Clear or preserve (configurable) |
| Sort change | Preserve selection |
| Refresh | Preserve selection |
| Modal open/close | Preserve selection |

## Bulk Operations

### Confirm Bulk Actions

```
┌─────────────── Delete Products ───────────────┐
│                                               │
│ Are you sure you want to delete 3 products?   │
│                                               │
│ • Widget Pro                                  │
│ • Gadget Basic                                │
│ • Tool Master                                 │
│                                               │
│ ⚠ This action cannot be undone.              │
│                                               │
│              [Cancel]  [Delete 3 Products]    │
└───────────────────────────────────────────────┘
```

### Bulk Operation Progress

```
┌─────────────── Updating Products ───────────────┐
│                                                 │
│ [████████████░░░░░░░░░░░░]  45%                │
│                                                 │
│ Processing 45 of 100 products...               │
│                                                 │
│                    [Cancel]                     │
└─────────────────────────────────────────────────┘
```

### Bulk Operation Results

```
┌─────────────── Operation Complete ───────────────┐
│                                                  │
│ ✓ 98 products updated successfully               │
│ ⚠ 2 products could not be updated               │
│                                                  │
│ Failed items:                                    │
│ • Widget Pro - In use by active orders           │
│ • Gadget Basic - Permission denied               │
│                                                  │
│              [View Details]  [Close]             │
└──────────────────────────────────────────────────┘
```

## Select All Behavior

### Page vs All Results

```
☑ Select all checkbox clicked:

Option A: Select visible page only
"3 items on this page selected"
[Select all 150 products]

Option B: Provide choice
"Select all 25 on this page" or "Select all 150 products"
```

### Select All Warning

For large selections:

```
┌─────────────────────────────────────────────────────────┐
│ ⚠ You're about to select 1,500 products.               │
│                                                         │
│ This may affect performance. Consider filtering first.  │
│                                                         │
│              [Cancel]  [Select All 1,500]               │
└─────────────────────────────────────────────────────────┘
```

## Keyboard Navigation

| Key | Action |
|-----|--------|
| Space | Toggle selection of focused row |
| Ctrl+A | Select all |
| Ctrl+Shift+A | Deselect all |
| Shift+Click | Range select |
| Ctrl+Click | Toggle individual |
| Arrow keys | Move focus |
| Escape | Clear selection |

## Accessibility

- Checkboxes have proper labels
- Selection state announced to screen readers
- Focus visible on selectable items
- Keyboard fully supports all selection modes
- `aria-selected` on selected items
- Selection count announced on change

## Selection Patterns by Use Case

### View/Edit Single Item

```
Click row → Navigate to detail
OR
Click row → Show in side panel
```

### Compare Items

```
Select 2-3 items → [Compare] button appears
→ Opens comparison view
```

### Move/Copy Items

```
Select items → [Move to...]
→ Opens destination picker
→ Moves items
```

### Batch Edit

```
Select items → [Edit All]
→ Opens batch edit dialog
→ Changes apply to all selected
```

### Export Selected

```
Select items → [Export Selected]
→ Downloads CSV/Excel with selected items only
```

## Anti-Patterns

```
❌ Selection persists across unrelated pages
❌ No visual feedback for selection
❌ No way to see what's selected
❌ Bulk actions without confirmation
❌ Select all includes filtered-out items
❌ No keyboard support for selection
```
