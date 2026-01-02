# Client Application Design Guide

A framework-agnostic collection of UX patterns, architectural guidance, and best practices for building CRUD-based business applications.

## Purpose

This guide provides:
- **Universal principles** applicable to any UI framework (React, Vue, Angular, etc.)
- **Proven patterns** for common CRUD application scenarios
- **Architectural guidance** for scalable client applications
- **UX best practices** for business software

## Document Structure

```
client-design-guide/
├── README.md                    # This file
│
├── principles/                  # Core UX principles
│   ├── 01-user-centered-design.md
│   ├── 02-consistency.md
│   ├── 03-feedback-and-response.md
│   ├── 04-error-prevention.md
│   ├── 05-accessibility.md
│   └── 06-progressive-disclosure.md
│
├── patterns/                    # Reusable UI patterns
│   ├── 01-data-display.md       # Tables, lists, cards
│   ├── 02-data-entry.md         # Forms, inputs, validation
│   ├── 03-navigation.md         # Menus, breadcrumbs, routing
│   ├── 04-search-filter.md      # Search, filter, sort
│   ├── 05-selection.md          # Single, multi, bulk selection
│   └── 06-crud-workflows.md     # Create, read, update, delete flows
│
├── components/                  # Component design guidance
│   ├── 01-data-grid.md          # Table/grid component
│   ├── 02-forms.md              # Form architecture
│   ├── 03-dropdowns.md          # Select, combobox, multiselect
│   ├── 04-dialogs.md            # Modals, confirmations, alerts
│   ├── 05-notifications.md      # Toast, snackbar, banners
│   └── 06-loading-states.md     # Spinners, skeletons, progress
│
├── interactions/                # User interaction patterns
│   ├── 01-click-behaviors.md    # Click, double-click, context
│   ├── 02-keyboard.md           # Shortcuts, focus, tabbing
│   ├── 03-drag-drop.md          # Reordering, file upload
│   ├── 04-inline-editing.md     # Edit-in-place patterns
│   └── 05-bulk-operations.md    # Multi-item actions
│
└── architecture/                # Technical architecture
    ├── 01-state-management.md   # Client state patterns
    ├── 02-api-integration.md    # Backend communication
    ├── 03-caching.md            # Client-side caching
    ├── 04-error-handling.md     # Error boundaries, recovery
    ├── 05-performance.md        # Optimization strategies
    └── 06-security.md           # Client-side security
```

## How to Use This Guide

### For Designers
Start with **principles/** to understand the foundational UX concepts, then reference **patterns/** when designing specific features.

### For Developers
Start with **architecture/** for technical foundations, then use **components/** as implementation reference for specific UI elements.

### For Product Managers
Review **principles/** and **patterns/06-crud-workflows.md** to understand standard user journeys and feature expectations.

## Core Philosophy

### 1. Convention Over Configuration
Users shouldn't have to think about how to use your application. Standard patterns reduce cognitive load.

### 2. Immediate Feedback
Every user action should produce visible feedback within 100ms. Users need to know the system received their input.

### 3. Recoverable Actions
Design for mistakes. Provide undo, confirmation dialogs for destructive actions, and clear error recovery paths.

### 4. Progressive Complexity
Simple tasks should be simple. Advanced features should be discoverable but not intrusive.

### 5. Consistent Mental Models
Similar actions should work the same way throughout the application. A user who learns one part of your app should intuitively understand other parts.

## Quick Reference

### Data Display Patterns
| Pattern | Use When |
|---------|----------|
| Data Grid | Displaying structured records with sorting/filtering |
| Card List | Showing items with rich visual content |
| Tree View | Hierarchical data relationships |
| Master-Detail | Parent-child navigation with detail panel |

### Form Patterns
| Pattern | Use When |
|---------|----------|
| Single Page Form | Simple entities (< 10 fields) |
| Tabbed Form | Medium complexity with logical groupings |
| Wizard/Stepper | Complex multi-step processes |
| Inline Edit | Quick updates to existing data |

### Navigation Patterns
| Pattern | Use When |
|---------|----------|
| Sidebar Menu | Primary navigation, many top-level items |
| Top Navigation | Few top-level items, horizontal space available |
| Breadcrumbs | Deep hierarchies, need to show location |
| Tabs | Related content at same hierarchy level |

## Framework Implementations

While this guide is framework-agnostic, companion implementation guides exist for:

- **template-project/** - React + PrimeReact implementation
- (Future) Vue + Vuetify implementation
- (Future) Angular + Angular Material implementation

## Maintenance

See **MAINTENANCE.md** for:
- Adding new content
- Document templates and standards
- Review cycles and quality assurance
- Contribution guidelines
- Expansion roadmap

## Contributing

When adding to this guide:
1. Keep content framework-agnostic
2. Include "Why" not just "What"
3. Provide concrete examples
4. Reference industry standards where applicable
5. Consider accessibility in all recommendations

## Related Projects

This design guide is part of a suite of reusable project templates:

| Project | Description |
|---------|-------------|
| `template-project/` | TypeScript code generator templates |
| `client-design-guide/` | This guide - UX patterns and principles |
| `client-impersonation-react/` | Role impersonation template |

Each project is standalone and can be used independently.
