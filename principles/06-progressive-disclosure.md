# Progressive Disclosure

## Principle

Show users only what they need, when they need it. Start simple and reveal complexity gradually based on user needs and expertise.

## Core Concept

```
Level 1: Essential        → Visible immediately
Level 2: Common           → One click away
Level 3: Advanced         → Hidden until requested
Level 4: Rare/Admin       → Buried or separate screen
```

## Implementation Patterns

### 1. Staged Forms

Break complex forms into stages:

```
Step 1: Essential Fields
┌─────────────────────────────────┐
│ Product Name: [_______________] │
│ Category:     [Select...    ▼] │
│ Price:        [$_____]         │
│                                 │
│              [Next →]           │
└─────────────────────────────────┘

Step 2: Additional Details
┌─────────────────────────────────┐
│ Description:  [              ] │
│               [              ] │
│ SKU:          [_______________] │
│ Manufacturer: [Select...    ▼] │
│                                 │
│        [← Back]   [Next →]      │
└─────────────────────────────────┘

Step 3: Advanced Options
┌─────────────────────────────────┐
│ □ Track inventory               │
│ □ Allow backorders              │
│ □ Require shipping              │
│                                 │
│        [← Back]   [Save]        │
└─────────────────────────────────┘
```

### 2. Expandable Sections

Show primary content, expand for details:

```
┌─────────────────────────────────┐
│ ▼ Basic Information             │
│   Product Name: Widget Pro      │
│   Category: Electronics         │
│   Price: $29.99                 │
├─────────────────────────────────┤
│ ▶ Description                   │  ← Collapsed by default
├─────────────────────────────────┤
│ ▶ Specifications                │
├─────────────────────────────────┤
│ ▶ Advanced Settings             │
└─────────────────────────────────┘
```

### 3. "Show More" Pattern

Reveal additional options on demand:

```
Filter Products
├─ Category     [Select... ▼]
├─ Status       [All      ▼]
├─ Price Range  [$__] to [$__]
│
└─ [+ Show More Filters]
   │
   Expands to reveal:
   ├─ Manufacturer
   ├─ Date Added
   ├─ Stock Level
   └─ Tags
```

### 4. Inline Expansion

Expand rows for detail without navigation:

```
┌────────────────────────────────────────────────┐
│ Product          │ Category    │ Price │ ⋯    │
├────────────────────────────────────────────────┤
│ ▶ Widget Pro     │ Electronics │ $29.99│ Edit │
├────────────────────────────────────────────────┤
│ ▼ Gadget Basic   │ Electronics │ $19.99│ Edit │
│ ┌──────────────────────────────────────────┐   │
│ │ Description: A basic gadget for...       │   │
│ │ SKU: GAD-001                             │   │
│ │ Stock: 145 units                         │   │
│ │ Last Updated: Jan 15, 2024               │   │
│ └──────────────────────────────────────────┘   │
├────────────────────────────────────────────────┤
│ ▶ Gizmo Deluxe   │ Electronics │ $49.99│ Edit │
└────────────────────────────────────────────────┘
```

### 5. Contextual Actions

Show actions based on selection/context:

```
No selection:
┌─────────────────────────────────┐
│ [+ Create]  [Export]  [Import]  │
└─────────────────────────────────┘

With selection (5 items):
┌─────────────────────────────────────────────────┐
│ [+ Create]  [Export]  [Import]  │ 5 selected:   │
│                                 │ [Delete] [Move]│
└─────────────────────────────────────────────────┘
```

### 6. Tooltips for Details

Additional info without cluttering UI:

```
Status: Active (?)
              │
              └→ Tooltip: "Active products are
                 visible in the catalog and
                 available for purchase."
```

### 7. Tabbed Interfaces

Organize related but distinct content:

```
┌──────┬──────────┬──────────┬─────────┐
│ Info │ Pricing  │ Inventory│ History │
├──────┴──────────┴──────────┴─────────┤
│                                       │
│  Currently showing: Info tab          │
│                                       │
│  Product Name: Widget Pro             │
│  Category: Electronics                │
│                                       │
└───────────────────────────────────────┘
```

## Decision Framework

### What to Show Immediately

| Criteria | Example |
|----------|---------|
| Required for primary task | Name, Category, Price on product list |
| Frequently used | Search, common filters |
| Expected by users | Navigation, status |
| Critical information | Errors, warnings |

### What to Hide Initially

| Criteria | Example |
|----------|---------|
| Rarely used | Bulk import, advanced settings |
| Contextual | Bulk actions (show when selecting) |
| Secondary information | Audit fields, metadata |
| Expert features | API access, custom queries |

### What to Put on Separate Pages

| Criteria | Example |
|----------|---------|
| Complex workflows | Multi-step wizards |
| Admin functions | User management, system settings |
| Historical data | Full audit logs |
| Reports | Detailed analytics |

## Examples by Feature

### Data Grid

```
Always visible:     Key columns (5-7), search, basic filters
Expandable:         Row details, all columns option
Hidden:             Column chooser, advanced filters
Separate page:      Bulk import, column configuration
```

### Forms

```
Always visible:     Required fields, primary fields
Expandable:         Optional sections, advanced options
Hidden:             System fields, audit info
Separate page:      Related entity management
```

### Navigation

```
Always visible:     Primary navigation (5-7 items)
Expandable:         Sub-navigation, entity lists
Hidden:             Admin menu (role-based)
Separate page:      Settings, preferences
```

### Filters

```
Always visible:     2-3 most common filters
Expandable:         Additional filters
Hidden:             Saved filter management
Separate page:      Advanced query builder
```

## Anti-Patterns

### Too Much Hidden

```
❌ Hiding required functionality
❌ Too many clicks to reach common features
❌ Important information only in tooltips
❌ Essential actions buried in menus
```

### Too Little Hidden

```
❌ Overwhelming new users with options
❌ Cluttered interface with rarely-used features
❌ All settings visible on primary screens
❌ No hierarchy of information importance
```

### Inconsistent Disclosure

```
❌ Some forms have expandable sections, others don't
❌ Advanced filters work differently across grids
❌ Inconsistent "show more" behaviors
```

## Measuring Effectiveness

### Metrics

| Metric | Target |
|--------|--------|
| Time to complete primary task | Decreasing |
| Click depth for common actions | ≤3 clicks |
| Feature discovery rate | >70% for core features |
| User errors | Decreasing |

### User Signals

| Signal | Interpretation |
|--------|----------------|
| Users don't find features | Buried too deep |
| Users ignore advanced features | Appropriate hiding |
| Users ask "where is X?" | Discoverability issue |
| Users complain about clutter | Not enough hiding |

## Checklist

- [ ] Primary task completable without expanding anything
- [ ] Advanced features are findable but not intrusive
- [ ] Expandable sections remember state (where appropriate)
- [ ] Hidden features have clear discovery path
- [ ] New users can complete basic tasks easily
- [ ] Power users can access advanced features quickly
- [ ] Disclosure levels are consistent across similar features
