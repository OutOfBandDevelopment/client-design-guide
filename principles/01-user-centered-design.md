# User-Centered Design

## Principle

Design decisions should be driven by user needs, tasks, and goals—not by technical constraints or organizational preferences.

## Core Concepts

### Know Your Users

Before designing any feature, understand:
- **Who** are the primary users?
- **What** tasks are they trying to accomplish?
- **Where** do they use the application (environment, devices)?
- **When** do they use it (frequency, duration)?
- **Why** do they need this feature?

### User Personas for CRUD Applications

Typical users of business CRUD applications:

| Persona | Characteristics | Design Implications |
|---------|-----------------|---------------------|
| Power User | High frequency, keyboard-driven | Support shortcuts, bulk operations |
| Occasional User | Infrequent use, needs guidance | Clear affordances, helpful defaults |
| Data Entry Clerk | Repetitive tasks, speed matters | Tab order, validation feedback |
| Manager/Reviewer | Read-heavy, needs overview | Dashboards, filtering, exports |
| Mobile User | Touch interface, limited screen | Responsive design, touch targets |

### Task-Oriented Design

Design around tasks, not features:

```
❌ Feature-oriented: "Here's a Product table"
✅ Task-oriented: "Find and update product prices"
```

#### Task Analysis Framework

1. **Goal**: What is the user trying to achieve?
2. **Trigger**: What initiates this task?
3. **Steps**: What actions are required?
4. **Outcome**: How does the user know they succeeded?
5. **Frequency**: How often is this task performed?

### Example: Product Price Update Task

```
Goal: Update prices for products in a category
Trigger: Quarterly price review
Steps:
  1. Navigate to Products
  2. Filter by category
  3. Select products to update
  4. Apply new prices
  5. Save changes
Outcome: Confirmation message, updated prices visible
Frequency: Quarterly (low frequency = needs guidance)
```

## Design Implications

### Reduce Cognitive Load

Users have limited mental capacity. Reduce the thinking required:

| Technique | Implementation |
|-----------|----------------|
| Smart defaults | Pre-fill likely values |
| Recognition over recall | Show options, don't require memorization |
| Chunking | Group related fields |
| Progressive disclosure | Hide advanced options until needed |

### Support User Mental Models

Users have expectations based on prior experience:

```
✅ Match expectations:
   - "Save" icon looks like a floppy disk (even if outdated)
   - Red indicates danger/delete
   - Left side navigation
   - Forms submit with Enter key

❌ Don't surprise users:
   - Unconventional icon meanings
   - Unexpected navigation changes
   - Actions without confirmation
```

### Design for the 80% Case

Optimize for common scenarios:

```
If 80% of users:                  Then:
─────────────────────────────────────────────────
Create 1-5 items at a time     → Single-item form (not bulk import)
Filter by 2-3 fields           → Show common filters, hide advanced
Never use feature X            → Move to "Advanced" or remove
Always select same options     → Remember preferences
```

### Accommodate the 20%

Don't ignore edge cases, but don't let them drive primary design:

- Provide escape hatches (export to Excel for complex manipulation)
- Support keyboard shortcuts for power users
- Allow customization without forcing it

## Measuring User-Centeredness

### Usability Metrics

| Metric | Target | How to Measure |
|--------|--------|----------------|
| Task completion rate | >95% | User testing |
| Time on task | Decreasing | Analytics |
| Error rate | <5% | Error logging |
| User satisfaction | >4/5 | Surveys |
| Support tickets | Decreasing | Ticket tracking |

### Red Flags

Signs your design isn't user-centered:

- Users create workarounds (spreadsheets, notes)
- High training requirements
- Frequent support requests for same features
- Users avoid certain features
- "That's how the database works" as justification

## Application to CRUD Systems

### Create Operations
- Pre-fill with smart defaults
- Validate early and clearly
- Allow saving drafts for complex forms
- Confirm success with next-action options

### Read Operations
- Show most relevant columns by default
- Provide meaningful sorting/filtering
- Support search with forgiving matching
- Remember user's view preferences

### Update Operations
- Show current values clearly
- Highlight what changed
- Require confirmation for bulk updates
- Provide undo when possible

### Delete Operations
- Require explicit confirmation
- Show what will be affected
- Prefer soft delete with recovery option
- Consider "archive" as alternative

## Checklist

Before releasing a feature:

- [ ] Can users complete the primary task in <3 clicks?
- [ ] Are error messages actionable?
- [ ] Do defaults make sense for most users?
- [ ] Is the feature discoverable without documentation?
- [ ] Have you tested with actual users?
- [ ] Does it work for both novice and expert users?
