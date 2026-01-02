# Data Display Patterns

## Overview

Patterns for presenting collections of data to users in CRUD applications.

## Pattern Selection Guide

| Pattern | Best For | Records | Complexity |
|---------|----------|---------|------------|
| Data Grid/Table | Structured records | 10-10,000+ | Low-Medium |
| Card Grid | Visual items, thumbnails | 10-100 | Low |
| List View | Simple items | 10-1,000 | Low |
| Tree View | Hierarchical data | 10-500 | Medium |
| Master-Detail | Parent-child browsing | 10-1,000 | Medium |
| Timeline | Time-based events | 10-500 | Low |

## Data Grid Pattern

The primary pattern for CRUD applications.

### Structure

```
┌─────────────────────────────────────────────────────────────┐
│ Title                                      [Filter] [Export]│
├─────────────────────────────────────────────────────────────┤
│ [Search...                    ]  Active filters: Category ×│
├────────────────────────────────────────────────────────────┤
│ □ │ Name ↑        │ Category    │ Price   │ Status │ Actions│
├───┼───────────────┼─────────────┼─────────┼────────┼────────┤
│ □ │ Widget Pro    │ Electronics │ $29.99  │ Active │ ⋯      │
│ □ │ Gadget Basic  │ Electronics │ $19.99  │ Active │ ⋯      │
│ □ │ Tool Master   │ Hardware    │ $49.99  │ Draft  │ ⋯      │
├───────────────────────────────────────────────────────────┤
│ Showing 1-25 of 150                    [<] [1] 2 3 ... [>] │
└─────────────────────────────────────────────────────────────┘
```

### Required Features

| Feature | Purpose |
|---------|---------|
| Column headers | Identify data, enable sorting |
| Sorting | Reorder by any column |
| Pagination | Handle large datasets |
| Row actions | Quick access to CRUD operations |
| Selection | Enable bulk operations |
| Search | Find specific records |

### Optional Features

| Feature | When to Include |
|---------|-----------------|
| Filtering sidebar | Complex filtering needs |
| Column resize | Variable content width |
| Column reorder | User customization |
| Row expansion | Inline detail view |
| Export | Data extraction needs |
| Infinite scroll | Real-time data feeds |

### Column Design

```
Column Type       │ Alignment │ Sortable │ Filterable
─────────────────────────────────────────────────────
Text              │ Left      │ ✓        │ ✓
Number            │ Right     │ ✓        │ ✓
Currency          │ Right     │ ✓        │ ✓
Date              │ Left      │ ✓        │ ✓
Status/Boolean    │ Center    │ ✓        │ ✓
Actions           │ Center    │ ✗        │ ✗
```

## Card Grid Pattern

Best for visual content or when items have images.

### Structure

```
┌─────────────────────────────────────────────────────────────┐
│ Products                                    [Grid] [List]   │
├─────────────────────────────────────────────────────────────┤
│ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│ │  [Image]    │  │  [Image]    │  │  [Image]    │          │
│ │             │  │             │  │             │          │
│ │ Widget Pro  │  │ Gadget X    │  │ Tool Master │          │
│ │ $29.99      │  │ $19.99      │  │ $49.99      │          │
│ │ ★★★★☆      │  │ ★★★☆☆      │  │ ★★★★★      │          │
│ └─────────────┘  └─────────────┘  └─────────────┘          │
│                                                             │
│ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│ │  [Image]    │  │  [Image]    │  │  [Image]    │          │
│ │     ...     │  │     ...     │  │     ...     │          │
│ └─────────────┘  └─────────────┘  └─────────────┘          │
└─────────────────────────────────────────────────────────────┘
```

### When to Use

- Products with images
- Media galleries
- User profiles
- File browsers

### Card Content

```
┌─────────────────┐
│ Visual (Image)  │  ← Primary visual element
├─────────────────┤
│ Title           │  ← Primary identifier
│ Subtitle        │  ← Secondary info
│ Metadata        │  ← Price, date, status
├─────────────────┤
│ [Action][Action]│  ← Quick actions (optional)
└─────────────────┘
```

## List View Pattern

Simpler than grid, more detail than cards.

### Structure

```
┌─────────────────────────────────────────────────────────────┐
│ Recent Activity                                             │
├─────────────────────────────────────────────────────────────┤
│ ┌───┬───────────────────────────────────────────────┬─────┐ │
│ │ 📦│ Product "Widget Pro" created                  │ 2m  │ │
│ │   │ by John Smith                                 │     │ │
│ ├───┼───────────────────────────────────────────────┼─────┤ │
│ │ ✏️│ Category "Electronics" updated                │ 15m │ │
│ │   │ by Jane Doe                                   │     │ │
│ ├───┼───────────────────────────────────────────────┼─────┤ │
│ │ 🗑│ Product "Old Widget" deleted                  │ 1h  │ │
│ │   │ by Admin                                      │     │ │
│ └───┴───────────────────────────────────────────────┴─────┘ │
└─────────────────────────────────────────────────────────────┘
```

### When to Use

- Activity feeds
- Notifications
- Search results
- Simple entity lists

## Tree View Pattern

For hierarchical data.

### Structure

```
┌─────────────────────────────────────────────────────────────┐
│ Categories                                                  │
├─────────────────────────────────────────────────────────────┤
│ ▼ Electronics (45)                                          │
│   ├─ ▶ Computers (12)                                       │
│   ├─ ▼ Phones (18)                                          │
│   │   ├─ iPhone                                             │
│   │   ├─ Android                                            │
│   │   └─ Accessories                                        │
│   └─ ▶ Tablets (15)                                         │
│ ▶ Clothing (89)                                             │
│ ▶ Home & Garden (34)                                        │
└─────────────────────────────────────────────────────────────┘
```

### When to Use

- Category hierarchies
- Organizational structures
- File systems
- Menu editors

### Interaction

| Action | Behavior |
|--------|----------|
| Click arrow | Expand/collapse |
| Click node | Select |
| Double-click | Expand AND select |
| Drag | Reorder (if enabled) |

## Master-Detail Pattern

Browse list while viewing detail.

### Structure

```
┌──────────────────┬──────────────────────────────────────────┐
│ Products         │ Widget Pro                               │
├──────────────────┤──────────────────────────────────────────┤
│ [Search...]      │ ┌────────────────────────────────────┐   │
│                  │ │  [Product Image]                   │   │
│ • Widget Pro   ◄─┼─┤                                    │   │
│ • Gadget Basic   │ │  Price: $29.99                     │   │
│ • Tool Master    │ │  Category: Electronics             │   │
│ • Part Standard  │ │  Status: Active                    │   │
│                  │ │                                    │   │
│                  │ │  Description:                      │   │
│                  │ │  A professional-grade widget...    │   │
│                  │ │                                    │   │
│                  │ └────────────────────────────────────┘   │
│                  │                                          │
│                  │         [Edit]  [Delete]  [Duplicate]    │
└──────────────────┴──────────────────────────────────────────┘
```

### When to Use

- Email clients
- Document management
- Quick browsing with preview
- Read-heavy interfaces

### Variations

| Variation | Layout |
|-----------|--------|
| Horizontal | List left, detail right |
| Vertical | List top, detail bottom |
| Modal detail | List full, detail in modal |

## Empty States

Every display pattern needs empty state handling.

### No Data (First Time)

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│                        📦                                   │
│                                                             │
│                 No products yet                             │
│                                                             │
│         Get started by creating your first product.        │
│                                                             │
│                   [+ Create Product]                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### No Results (Filtered)

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│                        🔍                                   │
│                                                             │
│              No products match your filters                 │
│                                                             │
│         Active filters: Category: "Widgets", Status: Draft │
│                                                             │
│                    [Clear Filters]                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Error State

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│                        ⚠️                                    │
│                                                             │
│                Unable to load products                      │
│                                                             │
│           There was a problem connecting to                 │
│           the server. Please try again.                     │
│                                                             │
│                       [Retry]                               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Performance Considerations

### Pagination vs Infinite Scroll

| Approach | Pros | Cons |
|----------|------|------|
| Pagination | Predictable, bookmarkable | Extra clicks |
| Infinite scroll | Smooth browsing | Hard to reach footer, memory |
| Virtual scroll | Best performance | Complex implementation |

### Lazy Loading

```
Load essential data immediately:
- First page of results
- Primary columns

Load on demand:
- Additional pages
- Detail panels
- Expanded rows
- Export data
```

## Responsive Behavior

### Grid to List

```
Desktop: Full grid with all columns
Tablet:  Reduced columns, same grid
Mobile:  Switch to card or list view
```

### Priority Columns

```
Always show:    Name/ID, primary identifier
Show on tablet: Key attributes, status
Show on desktop: All columns, actions
```
