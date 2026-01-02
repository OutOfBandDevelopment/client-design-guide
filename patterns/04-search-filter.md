# Search and Filter Patterns

## Overview

Patterns for helping users find specific data within large datasets.

## Search Patterns

### Basic Search

Single input field for quick search.

```
┌─────────────────────────────────────────┬───┐
│ 🔍 Search products...                   │   │
└─────────────────────────────────────────┴───┘
```

#### Search Behavior

| Behavior | Implementation |
|----------|----------------|
| Trigger | On Enter OR on input (debounced 300ms) |
| Scope | Search across key fields (name, ID, description) |
| Results | Highlight matching terms |
| Clear | X button when has value |

### Search with Suggestions

Autocomplete as user types.

```
┌─────────────────────────────────────────┬───┐
│ 🔍 wid                                  │ ✕ │
├─────────────────────────────────────────┴───┤
│ Products                                    │
│   Widget Pro                                │
│   Widget Basic                              │
│   Wide-angle Lens                           │
│ Categories                                  │
│   Widgets                                   │
│ Recent Searches                             │
│   widget accessories                        │
└─────────────────────────────────────────────┘
```

### Advanced Search

Structured search with multiple criteria.

```
┌─────────────────────────────────────────────────────────────┐
│ Advanced Search                                       [✕]   │
├─────────────────────────────────────────────────────────────┤
│ Product Name        Contains    ┌────────────────────────┐  │
│                                 │ widget                 │  │
│                                 └────────────────────────┘  │
│                                                             │
│ Category            Is          ┌────────────────────────┐  │
│                                 │ Electronics         ▼  │  │
│                                 └────────────────────────┘  │
│                                                             │
│ Price               Between     ┌────────┐ and ┌────────┐   │
│                                 │ $10    │     │ $100   │   │
│                                 └────────┘     └────────┘   │
│                                                             │
│ [+ Add Condition]                                           │
│                                                             │
│                          [Clear All]  [Search]              │
└─────────────────────────────────────────────────────────────┘
```

## Filter Patterns

### Filter Sidebar

Persistent filters alongside data.

```
┌────────────────────┬────────────────────────────────────────┐
│ Filters            │ Products                     25 results│
├────────────────────┼────────────────────────────────────────┤
│                    │                                        │
│ Status             │ ┌────────────────────────────────────┐ │
│ ☑ Active           │ │       Data Grid                    │ │
│ ☐ Inactive         │ │                                    │ │
│ ☐ Draft            │ │                                    │ │
│                    │ │                                    │ │
│ Category           │ │                                    │ │
│ ┌────────────────┐ │ │                                    │ │
│ │ All         ▼  │ │ └────────────────────────────────────┘ │
│ └────────────────┘ │                                        │
│                    │                                        │
│ Price Range        │                                        │
│ ├──────●────────┤  │                                        │
│ $0         $1000   │                                        │
│                    │                                        │
│ [Clear Filters]    │                                        │
└────────────────────┴────────────────────────────────────────┘
```

### Filter Bar

Inline filters above data.

```
┌─────────────────────────────────────────────────────────────┐
│ Products                                                    │
├─────────────────────────────────────────────────────────────┤
│ Status: [All     ▼]  Category: [All     ▼]  [More Filters] │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                     Data Grid                               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Active Filters Display

Show currently applied filters.

```
┌─────────────────────────────────────────────────────────────┐
│ Active Filters:  [Status: Active ✕]  [Category: Electronics ✕]│
│                                                [Clear All]  │
├─────────────────────────────────────────────────────────────┤
│                     Data Grid                               │
└─────────────────────────────────────────────────────────────┘
```

### Predefined Filters

Quick access to common filter combinations.

```
┌─────────────────────────────────────────────────────────────┐
│ [All Products]  [Active Only]  [Low Stock]  [New This Week] │
│      ↑                                                      │
│   Currently selected                                        │
├─────────────────────────────────────────────────────────────┤
│                     Data Grid                               │
└─────────────────────────────────────────────────────────────┘
```

## Filter Types

### Dropdown Filter

Single value selection.

```
Category
┌────────────────────────────┬───┐
│ All Categories             │ ▼ │
├────────────────────────────┴───┤
│ All Categories                 │
│ Electronics                    │
│ Clothing                       │
│ Home & Garden                  │
└────────────────────────────────┘
```

### Multi-Select Filter

Multiple value selection.

```
Categories
┌────────────────────────────────┐
│ ☑ Electronics                  │
│ ☑ Clothing                     │
│ ☐ Home & Garden                │
│ ☐ Sports                       │
└────────────────────────────────┘
Selected: Electronics, Clothing
```

### Range Filter

Numeric or date ranges.

```
Price Range
┌────────────┐  to  ┌────────────┐
│ $10        │      │ $100       │
└────────────┘      └────────────┘

Or with slider:
├─────────●═══════●─────────────┤
$0       $10      $100        $500
```

### Date Filter

```
Created Date
○ Any time
○ Today
○ This week
○ This month
● Custom range
  ┌──────────────┐ to ┌──────────────┐
  │ 01/01/2024 📅│    │ 01/31/2024 📅│
  └──────────────┘    └──────────────┘
```

### Boolean Filter

```
Stock Status
○ All
● In Stock only
○ Out of Stock only
```

### Text Filter

```
Product Name
Contains ▼ ┌──────────────────────┐
           │ widget               │
           └──────────────────────┘

Operators:
- Contains
- Starts with
- Ends with
- Equals
- Does not contain
```

## Search + Filter Combination

```
┌─────────────────────────────────────────────────────────────┐
│ ┌───────────────────────────────┐                           │
│ │ 🔍 Search...                  │    [Filters ▼]  [Export] │
│ └───────────────────────────────┘                           │
│                                                             │
│ Active: [Category: Electronics ✕] [Status: Active ✕]       │
├─────────────────────────────────────────────────────────────┤
│ Showing 15 of 150 products                                  │
│                                                             │
│                     Data Grid                               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## User Experience Guidelines

### Filter Behavior

| Action | Behavior |
|--------|----------|
| Apply filter | Immediate update (or Apply button for complex) |
| Clear single filter | Click X on filter chip |
| Clear all filters | "Clear All" button |
| Change filter | Replace previous value |

### Result Count

Always show how many results match:

```
✓ "25 products"
✓ "25 of 150 products" (filtered)
✓ "No products match your filters"
```

### Empty Results

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│                        🔍                                   │
│                                                             │
│            No products match your search                    │
│                                                             │
│   Suggestions:                                              │
│   • Check your spelling                                     │
│   • Try fewer filters                                       │
│   • Try different keywords                                  │
│                                                             │
│                  [Clear All Filters]                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Remember Filters

- Persist filter state in URL (for bookmarking/sharing)
- Optionally remember per user session
- Clear on explicit action only

### Performance

| Records | Approach |
|---------|----------|
| < 1,000 | Client-side filtering OK |
| 1,000 - 10,000 | Server-side, instant feedback |
| > 10,000 | Server-side with debounce/Apply button |

## Saved Filters

Allow users to save filter combinations.

```
┌─────────────────────────────────────────────────────────────┐
│ Saved Filters:  [My Active Products ▼]         [Save As...] │
├─────────────────────────────────────────────────────────────┤
│                     Data Grid                               │
└─────────────────────────────────────────────────────────────┘

Dropdown:
┌───────────────────────────────────────┐
│ Recent                                │
│   My Active Products                  │
│   Low Stock Items                     │
│ ─────────────────────────────────────│
│ Saved                                 │
│   ★ Electronics - Active              │
│   ★ Monthly Review                    │
│ ─────────────────────────────────────│
│ [+ Save Current Filters]              │
└───────────────────────────────────────┘
```

## Accessibility

- Filter controls have labels
- Screen reader announces result count changes
- Keyboard navigable filter controls
- Focus management when filters change
- Clear indication of active filters
