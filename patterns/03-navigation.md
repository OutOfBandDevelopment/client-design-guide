# Navigation Patterns

## Overview

Patterns for helping users move through the application and understand their location.

## Primary Navigation Patterns

### Sidebar Navigation

Most common for CRUD applications.

```
┌──────────────────┬───────────────────────────────────────────┐
│ App Logo         │ Page Title                     [User ▼]  │
├──────────────────┼───────────────────────────────────────────┤
│                  │                                           │
│ 🏠 Dashboard     │                                           │
│                  │                                           │
│ CATALOG          │          Page Content                     │
│ 📦 Products      │                                           │
│ 📁 Categories    │                                           │
│ 🏭 Manufacturers │                                           │
│                  │                                           │
│ ORDERS           │                                           │
│ 🛒 Orders        │                                           │
│ 👥 Customers     │                                           │
│                  │                                           │
│ SETTINGS         │                                           │
│ ⚙️ Configuration │                                           │
│ 👤 Users         │                                           │
│                  │                                           │
└──────────────────┴───────────────────────────────────────────┘
```

#### Sidebar Behaviors

| Behavior | Implementation |
|----------|----------------|
| Collapsible | Icon-only mode on collapse |
| Hover expand | Expand on hover when collapsed |
| Persistent state | Remember collapsed state |
| Active indicator | Highlight current section |

### Top Navigation

For applications with fewer primary sections.

```
┌─────────────────────────────────────────────────────────────┐
│ App Logo   Dashboard  Catalog  Orders  Settings   [User ▼] │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                                                             │
│                     Page Content                            │
│                                                             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### When to Use Top Nav

- 5-7 or fewer top-level items
- Wide screen applications
- Marketing/consumer-facing apps

### Combined Navigation

Top nav for primary sections, sidebar for sub-navigation.

```
┌─────────────────────────────────────────────────────────────┐
│ App Logo   Catalog  Orders  Settings               [User ▼]│
├────────────────┬────────────────────────────────────────────┤
│ Products       │                                            │
│ Categories     │         Page Content                       │
│ Manufacturers  │                                            │
│ Inventory      │                                            │
├────────────────┴────────────────────────────────────────────┤
```

## Secondary Navigation

### Breadcrumbs

Show hierarchical location.

```
Home > Catalog > Products > Edit Product

Implementation:
┌─────────────────────────────────────────────────────────────┐
│ Home / Catalog / Products / Widget Pro                      │
│                             └─ Current (not clickable)      │
│              └─ Clickable                                   │
└─────────────────────────────────────────────────────────────┘
```

#### Breadcrumb Rules

- Show for 3+ levels deep
- Last item is current page (not clickable)
- Truncate middle items for long paths: Home / ... / Products / Item
- Consider "Back" button instead for simple hierarchies

### Tabs

For related content at same level.

```
┌───────────────────────────────────────────────────────────┐
│ Product: Widget Pro                                       │
├──────────┬───────────┬────────────┬───────────┬──────────┤
│  Details │  Pricing  │  Inventory │  History  │  Related │
├──────────┴───────────┴────────────┴───────────┴──────────┤
│                                                           │
│  Tab Content                                              │
│                                                           │
└───────────────────────────────────────────────────────────┘
```

#### Tab Guidelines

- 5-7 tabs maximum
- Most important tab first (usually "Details")
- Persistent tab state (remember selected tab)
- URL reflects selected tab for bookmarking

### Sub-Navigation / Page Sections

For long pages with multiple sections.

```
┌───────────────────┬─────────────────────────────────────────┐
│ Jump to:          │ Product Details                         │
│                   ├─────────────────────────────────────────┤
│ • Basic Info      │ Basic Information                       │
│ • Description     │ ...                                     │
│ • Pricing         │                                         │
│ • SEO             │─────────────────────────────────────────│
│                   │ Description                             │
│                   │ ...                                     │
│                   │                                         │
│                   │─────────────────────────────────────────│
│                   │ Pricing                                 │
│                   │ ...                                     │
└───────────────────┴─────────────────────────────────────────┘
```

## Contextual Navigation

### Back Button

```
┌─────────────────────────────────────────────────────────────┐
│ ← Back to Products                                          │
│                                                             │
│ Edit Product                                                │
│ ...                                                         │
└─────────────────────────────────────────────────────────────┘
```

#### Back Button Rules

- Show when user navigated from a specific context
- Label with destination: "Back to Products" not just "Back"
- Use browser history when appropriate
- Don't show if user might have unsaved changes (handle separately)

### Related Links

```
┌─────────────────────────────────────────────────────────────┐
│ Product: Widget Pro                                         │
├─────────────────────────────────────────────────────────────┤
│ ...content...                                               │
├─────────────────────────────────────────────────────────────┤
│ Related:                                                    │
│ • View Category: Electronics                                │
│ • Edit Manufacturer: Acme Corp                              │
│ • See Orders containing this product                        │
└─────────────────────────────────────────────────────────────┘
```

### Action Links

Navigation to related actions.

```
Product saved successfully.

• View Product
• Create Another
• Back to List
```

## Navigation States

### Active/Current Indicator

```
Sidebar:
│ 📦 Products      │  ← Highlighted background, bold text

Tabs:
[Details]  Pricing   Inventory   ← Active tab styling

Breadcrumbs:
Home / Products / Widget Pro  ← Last item styled differently
```

### Hover State

```
│ 📦 Products      │  ← Normal
│ 📁 Categories    │  ← Hover: subtle highlight
```

### Disabled Navigation

```
│ 📦 Products      │  ← Active
│ 📊 Analytics     │  ← Disabled: grayed out, tooltip explains why
                       "Upgrade to access Analytics"
```

## Mobile Navigation

### Hamburger Menu

```
┌─────────────────────────────────────┐
│ ☰  App Name              [User ▼]  │
├─────────────────────────────────────┤
│                                     │
│         Page Content                │
│                                     │
└─────────────────────────────────────┘

When ☰ tapped:
┌──────────────────┬──────────────────┐
│ ✕ Menu           │                  │
├──────────────────┤                  │
│ 🏠 Dashboard     │     (dimmed      │
│ 📦 Products      │      content)    │
│ 📁 Categories    │                  │
│ ⚙️ Settings      │                  │
└──────────────────┴──────────────────┘
```

### Bottom Navigation

For key actions on mobile.

```
┌─────────────────────────────────────┐
│                                     │
│         Page Content                │
│                                     │
├─────────────────────────────────────┤
│  🏠      📦       ➕      👤       │
│ Home  Products  Create  Profile    │
└─────────────────────────────────────┘
```

## Navigation Best Practices

### Consistency

- Same navigation structure on all pages
- Same position for navigation elements
- Same icons/labels for same destinations

### Discoverability

- Important items visible without scrolling
- Logical grouping with clear labels
- Search for large navigation structures

### Efficiency

- Frequently used items easily accessible
- Keyboard shortcuts for power users
- Remember recent/favorite items

### Orientation

- Always show current location
- Breadcrumbs for deep hierarchies
- Page titles match navigation labels

## URL Structure

### RESTful URL Patterns

```
/products              → List
/products/123          → View (optional)
/products/123/edit     → Edit
/products/new          → Create

Alternative:
/products              → List
/products/edit/123     → Edit
/products/edit         → Create (new)
```

### Bookmarkable URLs

- Every distinct view has unique URL
- Tab selection reflected in URL
- Filter/sort state in URL (optional)
- Pagination in URL

```
/products?page=2&sort=name&filter=active
```

## Keyboard Navigation

| Key | Action |
|-----|--------|
| Tab | Move to next focusable element |
| Shift+Tab | Move to previous |
| Enter | Activate link/button |
| Escape | Close modal, cancel action |
| Arrow keys | Navigate within menus |
| Home/End | First/last menu item |

### Skip Links

```html
<a href="#main-content" class="skip-link">
  Skip to main content
</a>
```

Allows keyboard users to skip repetitive navigation.
