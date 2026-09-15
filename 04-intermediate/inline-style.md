# Inline Style

Use FSCSS directly in HTML `style` attributes. Quick, dynamic styling without separate files.

## Basic Syntax

Add FSCSS to `style` attributes:

```html
<div style="fscss: { background: #2563eb; color: white; padding: 1rem; }">
  Styled content
</div>
```

## Variables in Inline Style

Use variables directly:

```html
<style>
  $primary: #2563eb;
  $spacing: 1rem;
</style>

<div style="fscss: { background: $primary; padding: $spacing; }">
  Styled with variables
</div>
```

## Dynamic Styling

### Conditional Styles

```html
<div style="fscss: {
  @if $is-active {
    background: #2563eb;
    color: white;
  } @else {
    background: #f1f5f9;
    color: #334155;
  }
}">
  Dynamic content
</div>
```

### Loop-Based Styles

```html
<style>
  $colors: #2563eb, #64748b, #22c55e, #dc2626;
</style>

@for $i from 1 to 4 {
  <div style="fscss: { background: nth($colors, $i); }">
    Color block #{$i}
  </div>
}
```

## Practical Examples

### Hero Section

```html
<section style="fscss: {
  background: linear-gradient(135deg, #1e293b, #2563eb);
  padding: 4rem 2rem;
  text-align: center;
  color: white;
}">
  <h1 style="fscss: { font-size: 3rem; font-weight: 700; margin-bottom: 1rem; }">
    Welcome
  </h1>
  <p style="fscss: { font-size: 1.25rem; opacity: 0.9; }">
    Beautiful inline styling
  </p>
</section>
```

### Card Component

```html
<div style="fscss: {
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  padding: 1.5rem;
  max-width: 400px;
}">
  <h3 style="fscss: { font-size: 1.25rem; font-weight: 600; margin-bottom: 0.5rem; }">
    Card Title
  </h3>
  <p style="fscss: { color: #64748b; line-height: 1.6; }">
    Card content goes here.
  </p>
</div>
```

### Button Variants

```html
<style>
  $primary: #2563eb;
  $danger: #dc2626;
  $success: #22c55e;
</style>

<button style="fscss: {
  background: $primary;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  border: none;
  cursor: pointer;
}">
  Primary Button
</button>

<button style="fscss: {
  background: $danger;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  border: none;
  cursor: pointer;
}">
  Danger Button
</button>
```

## Inline Style vs Stylesheets

| Use Inline Style When... | Use Stylesheets When... |
|--------------------------|------------------------|
| Quick one-off styling | Repeated styles |
| Dynamic values | Static designs |
| Prototyping | Production code |
| Component-specific styles | Global themes |

## Best Practices

### 1. Keep It Simple

```html
<!-- Good: Simple inline style -->
<div style="fscss: { padding: 1rem; background: white; }">

<!-- Avoid: Complex inline styles -->
<div style="fscss: { /* 20 lines of complex styling */ }">
```

### 2. Use Variables

```html
<style>
  $primary: #2563eb;
</style>

<div style="fscss: { background: $primary; }">
```

### 3. Don't Repeat

If you use the same inline style multiple times, move it to a stylesheet:

```fscss
/* Better: Use a class instead */
.card {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
}
```

## Exercises

1. Create a hero section with inline FSCSS styles
2. Build a dynamic card component using inline variables
3. Design a button set with inline style variants

---

**Next: [Conditional Logic](./conditional-logic.md)**
