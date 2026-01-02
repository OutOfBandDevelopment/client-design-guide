# Data Grid Component

## Overview

The data grid is the primary component for displaying collections of structured data in CRUD applications. This document covers design decisions, features, and implementation guidance.

## Anatomy

```
┌─────────────────────────────────────────────────────────────────────┐
│ TOOLBAR                                                             │
│ Title              [Search      ]  [Filters ▼]  [Export]  [Create] │
├─────────────────────────────────────────────────────────────────────┤
│ ACTIVE FILTERS (when present)                                       │
│ [Status: Active ✕] [Category: Electronics ✕]         [Clear All]   │
├───┬─────────────────────────────────────────────────────────────────┤
│ H │ HEADER ROW                                                      │
│ D │ □ │ Name ↑         │ Category    │ Price ▼ │ Status │ Actions  │
├───┼───┼────────────────┼─────────────┼─────────┼────────┼──────────┤
│ B │ □ │ Widget Pro     │ Electronics │ $29.99  │ Active │ ⋯        │
│ O │ □ │ Gadget Basic   │ Electronics │ $19.99  │ Active │ ⋯        │
│ D │ ☑ │ Tool Master    │ Hardware    │ $49.99  │ Draft  │ ⋯        │
│ Y │ □ │ Part Standard  │ Hardware    │ $15.00  │ Active │ ⋯        │
├───┴───┴────────────────┴─────────────┴─────────┴────────┴──────────┤
│ FOOTER                                                              │
│ Showing 1-25 of 150 items          [<] [1] 2 3 4 ... 6 [>]         │
└─────────────────────────────────────────────────────────────────────┘
```

## Required Features

### Column Headers

| Feature | Behavior |
|---------|----------|
| Label | Clear, concise column name |
| Sort indicator | ↑ ascending, ↓ descending |
| Resize handle | Drag to resize (optional) |
| Reorder | Drag to reorder (optional) |

### Data Rows

| Feature | Behavior |
|---------|----------|
| Selection | Checkbox or highlight |
| Hover state | Subtle highlight |
| Focus state | Keyboard navigation indicator |
| Alternating colors | Optional for readability |

### Pagination

| Feature | Behavior |
|---------|----------|
| Page size | Default 25, options: 10, 25, 50, 100 |
| Page navigation | First, prev, numbered, next, last |
| Result count | "Showing 1-25 of 150" |
| Page input | Jump to specific page |

## Column Types

### Text Column

```
┌────────────────────────────────────┐
│ Product Name                       │
├────────────────────────────────────┤
│ Widget Professional Edition        │ ← Full text
│ Gadget Basic Model with Ext...     │ ← Truncated with tooltip
└────────────────────────────────────┘

Alignment: Left
Sortable: Yes
Filterable: Yes (contains, starts with)
```

### Numeric Column

```
┌──────────────┐
│        Price │
├──────────────┤
│       $29.99 │  ← Right aligned
│      $199.00 │
│        $5.50 │
└──────────────┘

Alignment: Right
Sortable: Yes
Filterable: Yes (equals, range)
Format: Currency, decimal places, thousands separator
```

### Date Column

```
┌──────────────────┐
│ Created          │
├──────────────────┤
│ Jan 15, 2024     │  ← Formatted date
│ Dec 3, 2023      │
│ Nov 28, 2023     │
└──────────────────┘

Alignment: Left
Sortable: Yes
Filterable: Yes (date range)
Format: Localized date format
Tooltip: Full date/time on hover
```

### Status Column

```
┌──────────────┐
│ Status       │
├──────────────┤
│ [Active    ] │  ← Tag/badge style
│ [Pending   ] │
│ [Inactive  ] │
└──────────────┘

Alignment: Center
Sortable: Yes
Filterable: Yes (multi-select)
Colors: Green=active, Yellow=pending, Red=inactive
```

### Boolean Column

```
┌──────────┐
│ In Stock │
├──────────┤
│    ✓     │  ← Icon or tag
│    ✕     │
│    ✓     │
└──────────┘

Alignment: Center
Sortable: Yes
Filterable: Yes (checkbox)
Display: Icon, checkbox, or Yes/No text
```

### Action Column

```
┌──────────────────────┐
│ Actions              │
├──────────────────────┤
│ [👁] [✏️] [🗑]       │  ← Icon buttons
│ [⋯]                  │  ← Overflow menu
│ [Edit] [More ▼]      │  ← Text + menu
└──────────────────────┘

Alignment: Center or Right
Sortable: No
Fixed width: Yes
Position: Usually last column
```

### Link Column

```
┌────────────────────────────────────┐
│ Order Number                       │
├────────────────────────────────────┤
│ ORD-2024-001                       │  ← Clickable link
│ ORD-2024-002                       │
└────────────────────────────────────┘

Styling: Underline or primary color
Click: Navigate to detail view
```

## Toolbar Features

### Search

```
┌─────────────────────────────────┐
│ 🔍 Search products...           │
└─────────────────────────────────┘

Behavior:
- Debounced (300ms)
- Searches key fields (configurable)
- Clear button when has value
- Optional: advanced search toggle
```

### Filter Button

```
[Filters ▼] or [🔧 Filters]

Opens:
- Sidebar panel (recommended for complex)
- Dropdown panel (for simple filters)
- Modal (for very complex filters)
```

### Export

```
[Export ▼]
├─ Export to Excel
├─ Export to CSV
└─ Export visible columns only

Options:
- Export all vs. selected
- Include/exclude columns
- Custom filename
```

### Column Management

```
[Columns ▼]
├─ ☑ Name
├─ ☑ Category
├─ ☐ SKU (hidden)
├─ ☑ Price
└─ [Reset to Default]
```

## Selection Modes

### None

No selection, rows are purely informational.

### Single

Click row to select. Used for master-detail or navigation.

### Multiple (Checkbox)

Checkbox column for multi-select. Enables bulk operations.

```
Header checkbox states:
☐ = None selected
☑ = All selected
▣ = Some selected (indeterminate)
```

## Responsive Behavior

### Desktop (>1200px)

Full grid with all features.

### Tablet (768-1200px)

- Reduce visible columns
- Horizontal scroll for remaining
- Consider hiding filters in sidebar

### Mobile (<768px)

```
Option A: Card Layout
┌─────────────────────┐
│ Widget Pro          │
│ Electronics | $29.99│
│ Status: Active      │
│ [Edit] [Delete]     │
└─────────────────────┘

Option B: Single Column
┌─────────────────────────────────┐
│ Name: Widget Pro                │
│ Category: Electronics           │
│ Price: $29.99                   │
│ Status: Active                  │
│ [Actions ▼]                     │
├─────────────────────────────────┤
│ Name: Gadget Basic              │
│ ...                             │
└─────────────────────────────────┘
```

## Loading States

### Initial Load

```
┌─────────────────────────────────────────────────────────────┐
│ Products                                                    │
├─────────────────────────────────────────────────────────────┤
│ ████████  │ ██████  │ ████  │ ██████  │                    │
│ ██████    │ ████████│ ██    │ ████    │                    │
│ ████████  │ ██████  │ ████  │ ██████  │                    │
│ ██████████│ ████    │ ██████│ ████    │                    │
└─────────────────────────────────────────────────────────────┘
Skeleton rows showing expected layout
```

### Refresh/Filter

```
┌─────────────────────────────────────────────────────────────┐
│ Products                                         Loading... │
├─────────────────────────────────────────────────────────────┤
│ (dimmed existing content with spinner overlay)              │
└─────────────────────────────────────────────────────────────┘
```

## Empty States

### No Data

```
┌─────────────────────────────────────────────────────────────┐
│                         📦                                  │
│                                                             │
│               No products found                             │
│                                                             │
│      Get started by creating your first product.           │
│                                                             │
│                  [+ Create Product]                         │
└─────────────────────────────────────────────────────────────┘
```

### No Results (Filtered)

```
┌─────────────────────────────────────────────────────────────┐
│                         🔍                                  │
│                                                             │
│          No products match your filters                     │
│                                                             │
│      Active filters: Status=Draft, Category=Electronics    │
│                                                             │
│                   [Clear Filters]                           │
└─────────────────────────────────────────────────────────────┘
```

## Accessibility

| Requirement | Implementation |
|-------------|----------------|
| Keyboard navigation | Arrow keys between cells |
| Sort announcement | "Sorted by Name, ascending" |
| Selection announcement | "Row 3 selected" |
| Column headers | `<th scope="col">` |
| Row headers | `<th scope="row">` for first cell |
| Focus visible | Clear focus indicator |
| Screen reader | ARIA grid role |

## Performance

| Data Size | Strategy |
|-----------|----------|
| < 100 | Load all, client-side |
| 100 - 10,000 | Server pagination |
| > 10,000 | Virtual scrolling |

### Optimizations

- Debounce search/filter (300ms)
- Memoize column definitions
- Virtualize for large datasets
- Lazy load row expansion content
