# CRUD Workflows

## Overview

Standard user flows for Create, Read, Update, and Delete operations in business applications.

## Create Workflow

### Standard Create Flow

```
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│  List   │───▶│  Form   │───▶│ Validate│───▶│ Success │
│  Page   │    │  (New)  │    │  & Save │    │  State  │
└─────────┘    └─────────┘    └─────────┘    └─────────┘
     │              │              │              │
     │         [Cancel]       [Errors]      [Options]
     │              │              │              │
     ▼              ▼              ▼              ▼
   Click      Return to       Show inline    • View item
  "Create"      list         validation     • Create another
                                            • Return to list
```

### Create Entry Points

```
1. Toolbar Button (Primary)
┌─────────────────────────────────────────────────────────────┐
│ Products                                       [+ Create]   │
└─────────────────────────────────────────────────────────────┘

2. Empty State
┌─────────────────────────────────────────────────────────────┐
│                    No products yet                          │
│                [+ Create First Product]                     │
└─────────────────────────────────────────────────────────────┘

3. Quick Create (Inline)
┌─────────────────────────────────────────────────────────────┐
│ Products                                                    │
├─────────────────────────────────────────────────────────────┤
│ + Add new product... │                │         │          │
├──────────────────────┼────────────────┼─────────┼──────────┤
│ Widget Pro           │ Electronics    │ $29.99  │ Edit     │
└─────────────────────────────────────────────────────────────┘
```

### Create Form Patterns

```
Simple Entity (< 5 fields):
→ Inline form or modal

Medium Entity (5-15 fields):
→ Full page form

Complex Entity (15+ fields):
→ Multi-step wizard OR
→ Tabbed form
```

### Post-Create Actions

```
┌─────────────────────────────────────────────────────────────┐
│ ✓ Product created successfully                              │
│                                                             │
│   • View "Widget Pro"                                       │
│   • Create another product                                  │
│   • Return to products list                                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Read Workflow

### List View (Browse)

```
┌─────────────────────────────────────────────────────────────┐
│ Products                                      [+ Create]    │
├─────────────────────────────────────────────────────────────┤
│ [Search...              ]    [Filters ▼]      [Export]     │
├─────────────────────────────────────────────────────────────┤
│ Name          │ Category     │ Price   │ Status │ Actions  │
├───────────────┼──────────────┼─────────┼────────┼──────────┤
│ Widget Pro    │ Electronics  │ $29.99  │ Active │ ⋯        │
│ ...           │              │         │        │          │
├─────────────────────────────────────────────────────────────┤
│ Showing 1-25 of 150                    [<] 1 2 3 ... [>]   │
└─────────────────────────────────────────────────────────────┘
```

### Detail View

```
Option A: Full Page Detail
┌─────────────────────────────────────────────────────────────┐
│ ← Back to Products                                          │
├─────────────────────────────────────────────────────────────┤
│ Widget Pro                              [Edit] [Delete]     │
├─────────────────────────────────────────────────────────────┤
│ Category:     Electronics                                   │
│ Price:        $29.99                                        │
│ Status:       Active                                        │
│ Description:  A professional-grade widget...                │
│ Created:      Jan 15, 2024 by John Smith                    │
└─────────────────────────────────────────────────────────────┘

Option B: Side Panel Detail
┌───────────────────────────────┬─────────────────────────────┐
│ Products              [+ New] │ Widget Pro          [Edit]  │
├───────────────────────────────┼─────────────────────────────┤
│ • Widget Pro ←               │ Category: Electronics       │
│ • Gadget Basic                │ Price: $29.99              │
│ • Tool Master                 │ Status: Active             │
└───────────────────────────────┴─────────────────────────────┘

Option C: Expandable Row
┌─────────────────────────────────────────────────────────────┐
│ ▼ Widget Pro     │ Electronics  │ $29.99  │ Active │ Edit  │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Description: A professional-grade widget...             │ │
│ │ SKU: WGT-001  │  Created: Jan 15, 2024                  │ │
│ └─────────────────────────────────────────────────────────┘ │
│ ▶ Gadget Basic   │ Electronics  │ $19.99  │ Active │ Edit  │
└─────────────────────────────────────────────────────────────┘
```

### View vs Edit Mode

```
View Mode:
- Read-only display
- Focus on presentation
- Quick actions available

Edit Mode:
- Editable fields
- Save/Cancel buttons
- Validation active
```

## Update Workflow

### Standard Update Flow

```
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│  View/  │───▶│  Edit   │───▶│ Validate│───▶│ Success │
│  List   │    │  Form   │    │  & Save │    │  State  │
└─────────┘    └─────────┘    └─────────┘    └─────────┘
                    │              │
               [Cancel]       [Errors]
                    │              │
                    ▼              ▼
               Confirm        Show inline
               discard?       validation
```

### Edit Entry Points

```
1. Row Action
┌────────────────────────────────────────────────────────────┐
│ Widget Pro    │ Electronics  │ $29.99  │ [Edit] [Delete]  │
└────────────────────────────────────────────────────────────┘

2. Detail View Action
┌────────────────────────────────────────────────────────────┐
│ Widget Pro                              [Edit] [Delete]    │
└────────────────────────────────────────────────────────────┘

3. Inline Edit (double-click or edit icon)
┌────────────────────────────────────────────────────────────┐
│ [Widget Pro     ]│ Electronics  │ [$29.99 ]│ [✓] [✗]      │
└────────────────────────────────────────────────────────────┘
```

### Inline Editing

For simple, quick edits:

```
Normal:     Widget Pro        │ $29.99
Click:      [Widget Pro     ] │ [$29.99 ]
                              │ [✓ Save] [✗ Cancel]
```

### Edit Conflict Handling

When another user edited the same record:

```
┌─────────────────────────────────────────────────────────────┐
│ ⚠ This product was modified by Jane Doe at 2:34 PM        │
│                                                             │
│ Your changes:          Server version:                      │
│ Price: $29.99          Price: $34.99                        │
│                                                             │
│ [Keep Mine]  [Use Server Version]  [Review Differences]     │
└─────────────────────────────────────────────────────────────┘
```

### Bulk Update

```
┌─────────────── Update 5 Products ───────────────┐
│                                                 │
│ Fields to update (leave blank to keep current): │
│                                                 │
│ Category:  [Electronics        ▼]               │
│ Status:    [No change          ▼]               │
│ Price:     [                    ]               │
│                                                 │
│ Preview: 5 products will be updated             │
│                                                 │
│              [Cancel]  [Update 5 Products]      │
└─────────────────────────────────────────────────┘
```

## Delete Workflow

### Standard Delete Flow

```
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│  View/  │───▶│ Confirm │───▶│ Execute │───▶│ Success │
│  List   │    │ Dialog  │    │ Delete  │    │ Feedback│
└─────────┘    └─────────┘    └─────────┘    └─────────┘
                    │                             │
               [Cancel]                    [Undo option]
```

### Delete Confirmation

```
Simple Delete:
┌─────────────── Delete Product ───────────────┐
│                                              │
│ Delete "Widget Pro"?                         │
│                                              │
│ This action cannot be undone.                │
│                                              │
│              [Cancel]  [Delete]              │
└──────────────────────────────────────────────┘

Delete with Dependencies:
┌─────────────── Delete Category ───────────────┐
│                                               │
│ Delete "Electronics"?                         │
│                                               │
│ ⚠ This category contains:                    │
│ • 45 products                                 │
│ • 3 subcategories                             │
│                                               │
│ What should happen to these items?            │
│ ○ Move to another category                    │
│ ● Delete everything                           │
│                                               │
│               [Cancel]  [Delete]              │
└───────────────────────────────────────────────┘
```

### High-Stakes Delete

Require explicit confirmation:

```
┌──────────────── Delete 150 Products ────────────────┐
│                                                     │
│ ⚠ You are about to delete 150 products            │
│                                                     │
│ This action is permanent and cannot be undone.      │
│ Related orders and inventory will be affected.      │
│                                                     │
│ Type "DELETE" to confirm:                           │
│ ┌─────────────────────────────────────────────────┐ │
│ │                                                 │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│                [Cancel]  [Delete]                   │
└─────────────────────────────────────────────────────┘
```

### Soft Delete / Archive

```
┌─────────────── Archive Product ───────────────┐
│                                               │
│ Archive "Widget Pro"?                         │
│                                               │
│ Archived products:                            │
│ • Won't appear in catalogs                    │
│ • Can be restored later                       │
│ • Historical data is preserved                │
│                                               │
│              [Cancel]  [Archive]              │
└───────────────────────────────────────────────┘
```

### Undo Delete

```
┌─────────────────────────────────────────────────────────────┐
│ ✓ Product deleted                                [Undo]     │
└─────────────────────────────────────────────────────────────┘
                                                   ↑
                                           Available for
                                           10 seconds
```

## Error Handling

### Validation Errors

```
┌─────────────────────────────────────────────────────────────┐
│ ⚠ Please correct the following errors:                     │
│                                                             │
│ • Product name is required                                  │
│ • Price must be greater than 0                              │
└─────────────────────────────────────────────────────────────┘

Product Name *
┌─────────────────────────────────────────────────────────────┐
│                                                             │
└─────────────────────────────────────────────────────────────┘
⚠ Product name is required
```

### Save Errors

```
┌─────────────────────────────────────────────────────────────┐
│ ⚠ Unable to save product                                   │
│                                                             │
│ A product with this name already exists.                    │
│                                                             │
│ [Change Name]  [View Existing]                              │
└─────────────────────────────────────────────────────────────┘
```

### Delete Errors

```
┌─────────────────────────────────────────────────────────────┐
│ ⚠ Unable to delete product                                 │
│                                                             │
│ This product is referenced by 3 active orders.              │
│ Complete or cancel these orders first.                      │
│                                                             │
│ [View Orders]  [Cancel]                                     │
└─────────────────────────────────────────────────────────────┘
```

## Workflow Summary

| Operation | Entry Point | Confirmation | Success Feedback |
|-----------|-------------|--------------|------------------|
| Create | Button/Link | None (on save) | Toast + Options |
| Read | Click/Navigate | None | Content display |
| Update | Edit action | On cancel with changes | Toast |
| Delete | Delete action | Always | Toast + Undo |
