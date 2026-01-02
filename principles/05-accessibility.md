# Accessibility

## Principle

Applications must be usable by everyone, including people with disabilities. Accessible design benefits all users—it's not just compliance, it's good design.

## Core Requirements (WCAG 2.1)

### Level A (Minimum)

| Requirement | Implementation |
|-------------|----------------|
| Text alternatives | Alt text for images |
| Keyboard accessible | All functions via keyboard |
| No seizure triggers | No flashing content |
| Navigable | Skip links, focus order |

### Level AA (Recommended)

| Requirement | Implementation |
|-------------|----------------|
| Color contrast | 4.5:1 for text, 3:1 for UI |
| Resize text | Up to 200% without loss |
| Multiple ways | Search + navigation |
| Focus visible | Clear focus indicators |
| Error identification | Clear error messages |

## Keyboard Accessibility

### Essential Keyboard Support

Every interactive element must be:
- **Focusable** with Tab
- **Activatable** with Enter/Space
- **Dismissable** with Escape (modals, dropdowns)

### Standard Keyboard Patterns

| Component | Keyboard Behavior |
|-----------|-------------------|
| Button | Enter/Space to activate |
| Link | Enter to follow |
| Checkbox | Space to toggle |
| Radio group | Arrow keys to move |
| Dropdown | Arrows to navigate, Enter to select |
| Menu | Arrows to navigate, Enter to activate |
| Modal | Tab trapped, Escape to close |
| Data grid | Arrows to navigate cells |

### Focus Management

```
Focus order should match visual order (left-to-right, top-to-bottom)

Tab Order:
[1] Search input
[2] Filter button
[3] First table header
[4] First table row
...

Focus trapping (for modals):
- Tab cycles within modal
- Escape closes modal
- Focus returns to trigger element
```

### Skip Links

Allow users to skip repetitive navigation:

```html
<!-- First focusable element -->
<a href="#main-content" class="skip-link">
  Skip to main content
</a>

<nav>...</nav>

<main id="main-content">
  <!-- Main content here -->
</main>
```

## Visual Accessibility

### Color Contrast

| Element | Minimum Ratio |
|---------|---------------|
| Normal text | 4.5:1 |
| Large text (18px+ or 14px bold) | 3:1 |
| UI components, graphics | 3:1 |

#### Testing Contrast

```
Use tools:
- Chrome DevTools (Accessibility tab)
- WebAIM Contrast Checker
- Axe browser extension

Common failures:
- Light gray text on white: #999 on #fff = 2.8:1 ❌
- Fix: Use #767676 or darker = 4.5:1 ✓
```

### Color Independence

Never rely on color alone:

```
❌ Bad:
   Red = Error, Green = Success (colorblind users can't distinguish)

✅ Good:
   🔴 Error icon + "Error" text
   ✅ Check icon + "Success" text
   Different shapes + colors + text
```

#### Status Indicators

```
✓ Success    (green + checkmark + "Success" text)
⚠ Warning    (yellow + warning icon + "Warning" text)
✕ Error      (red + X icon + "Error" text)
ℹ Info       (blue + info icon + "Info" text)
```

### Focus Indicators

Visible focus for all interactive elements:

```css
/* Minimum focus indicator */
:focus {
  outline: 2px solid #005fcc;
  outline-offset: 2px;
}

/* Enhanced for better visibility */
:focus-visible {
  outline: 3px solid #005fcc;
  outline-offset: 2px;
  box-shadow: 0 0 0 4px rgba(0, 95, 204, 0.3);
}
```

### Text Sizing

Support text zoom up to 200%:

```css
/* Use relative units */
font-size: 1rem;      /* ✓ */
font-size: 16px;      /* ✗ Doesn't scale with user preferences */

/* Ensure containers expand */
min-height: 2em;      /* ✓ Scales with text */
height: 32px;         /* ✗ Fixed, may clip */
```

## Screen Reader Support

### Semantic HTML

Use correct HTML elements:

```html
<!-- ✓ Correct -->
<button>Save</button>
<a href="/products">Products</a>
<nav><ul><li>...</li></ul></nav>
<table><thead>...</thead><tbody>...</tbody></table>

<!-- ✗ Wrong -->
<div onclick="save()">Save</div>
<span onclick="navigate()">Products</span>
<div class="nav">...</div>
<div class="table">...</div>
```

### ARIA Labels

When semantic HTML isn't sufficient:

```html
<!-- Icon-only button -->
<button aria-label="Delete product">
  <DeleteIcon />
</button>

<!-- Custom component -->
<div role="combobox"
     aria-expanded="false"
     aria-haspopup="listbox"
     aria-label="Select category">
  ...
</div>

<!-- Data grid -->
<div role="grid" aria-label="Products">
  <div role="row">
    <div role="columnheader">Name</div>
    <div role="columnheader">Price</div>
  </div>
  <div role="row">
    <div role="gridcell">Widget</div>
    <div role="gridcell">$9.99</div>
  </div>
</div>
```

### Live Regions

Announce dynamic content:

```html
<!-- For toast notifications -->
<div role="alert" aria-live="polite">
  Product saved successfully
</div>

<!-- For loading states -->
<div aria-live="polite" aria-busy="true">
  Loading products...
</div>

<!-- For form errors -->
<div role="alert" aria-live="assertive">
  Please correct the errors below
</div>
```

### Form Labels

Every input needs a label:

```html
<!-- Visible label -->
<label for="email">Email</label>
<input id="email" type="email" />

<!-- Hidden label (icon-only input) -->
<label for="search" class="visually-hidden">Search products</label>
<input id="search" type="search" placeholder="Search..." />

<!-- aria-label alternative -->
<input type="search" aria-label="Search products" />
```

## Component Accessibility

### Data Grids

```html
<table role="grid" aria-label="Products">
  <thead>
    <tr>
      <th scope="col" aria-sort="ascending">Name</th>
      <th scope="col">Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Widget</td>
      <td>$9.99</td>
    </tr>
  </tbody>
</table>
```

Keyboard: Arrow keys navigate cells, Enter activates actions.

### Modals

```html
<div role="dialog"
     aria-modal="true"
     aria-labelledby="dialog-title"
     aria-describedby="dialog-description">
  <h2 id="dialog-title">Confirm Delete</h2>
  <p id="dialog-description">This action cannot be undone.</p>
  <button>Cancel</button>
  <button>Delete</button>
</div>
```

Requirements:
- Focus trapped inside modal
- Escape closes modal
- Focus returns to trigger on close

### Dropdowns/Combobox

```html
<div role="combobox"
     aria-expanded="false"
     aria-haspopup="listbox"
     aria-controls="listbox-id">
  <input aria-autocomplete="list" />
</div>

<ul role="listbox" id="listbox-id">
  <li role="option" aria-selected="false">Option 1</li>
  <li role="option" aria-selected="true">Option 2</li>
</ul>
```

## Testing Accessibility

### Automated Testing

| Tool | What It Catches |
|------|-----------------|
| axe DevTools | Missing labels, contrast, ARIA errors |
| Lighthouse | General accessibility audit |
| WAVE | Visual accessibility indicators |

### Manual Testing

| Test | How |
|------|-----|
| Keyboard only | Unplug mouse, complete all tasks |
| Screen reader | Use NVDA (Windows), VoiceOver (Mac) |
| Zoom 200% | Browser zoom, check layout |
| High contrast | Windows High Contrast mode |

### Testing Checklist

- [ ] All functionality available via keyboard
- [ ] Focus visible on all interactive elements
- [ ] Tab order follows visual order
- [ ] Images have alt text
- [ ] Form fields have labels
- [ ] Errors announced to screen readers
- [ ] Color contrast meets 4.5:1
- [ ] Works at 200% zoom
- [ ] No information conveyed by color alone

## Common Mistakes

```
❌ Div with onclick instead of button
❌ Placeholder as only label
❌ Focus outline removed for aesthetics
❌ Form errors only indicated by color
❌ Modal doesn't trap focus
❌ Custom components without ARIA
❌ Images without alt text
❌ Auto-playing media without controls
```
