# Patterns

Common design patterns implemented in FSCSS. Learn reusable solutions for everyday UI challenges.

## BEM Naming Pattern

Block Element Modifier — a methodology for naming CSS classes:

```fscss
/* Block */
.card { ... }

/* Element */
.card__header { ... }
.card__body { ... }
.card__footer { ... }

/* Modifier */
.card--primary { ... }
.card--large { ... }
```

### BEM in FSCSS

```fscss
@define card-variant(bg, text) {
  background: @use(bg);
  color: @use(text);
}

.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.card__header {
  padding: 1.5rem;
  border-bottom: 1px solid #e2e8f0;
}

.card__body {
  padding: 1.5rem;
}

.card__footer {
  padding: 1.5rem;
  border-top: 1px solid #e2e8f0;
}

.card--primary {
  @card-variant(#2563eb, white);
}

.card--danger {
  @card-variant(#dc2626, white);
}
```

## OOCSS Pattern

Object-Oriented CSS — separate structure from skin:

```fscss
/* Structure (the "object") */
@fun(media-object) {
  display: flex;
  align-items: flex-start;
}

@fun(media-body) {
  flex: 1;
  padding: $spacing-md;
}

@fun(media-img) {
  width: 64px;
  height: 64px;
  border-radius: $radius-md;
}

/* Skin (the "theme") */
@fun(media-light) {
  background: white;
  border: 1px solid #e2e8f0;
}

@fun(media-dark) {
  background: #1e293b;
  color: white;
}

/* Usage */
.media { @fun.media-object; @fun.media-light; }
.media__img { @fun.media-img; }
.media__body { @fun.media-body; }
```

## SMACSS Pattern

Scalable and Modular Architecture for CSS — categorize styles:

```fscss
/* Base */
body { font-family: $font-family; }
a { color: $primary; }

/* Layout */
.container { max-width: 1200px; margin: 0 auto; }
.header { /* ... */ }
.footer { /* ... */ }

/* Module */
.card { /* ... */ }
.button { /* ... */ }
.input { /* ... */ }

/* State */
.is-hidden { display: none; }
.is-active { color: $primary; font-weight: 600; }
.is-disabled { opacity: 0.5; pointer-events: none; }

/* Theme */
.theme-dark {
  background: #0f172a;
  color: white;
}
```

## Component Pattern

Build self-contained, reusable components:

```fscss
/* Component definition */
@define badge(size, bg, text) {
  display: inline-block;
  padding: @if size == sm { 4px 8px; } @else { 8px 16px; }
  background: @use(bg);
  color: @use(text);
  border-radius: $radius-full;
  font-weight: 600;
  font-size: @if size == sm { 12px; } @else { 14px; }
}

/* Component usage */
.badge { @badge(md, $primary, white); }
.badge-sm { @badge(sm, $secondary, white); }
.badge-success { @badge(md, $success, white); }
.badge-danger { @badge(md, $danger, white); }
```

## Utility Pattern

Create single-purpose utility classes:

```fscss
/* Display */
.d-none { display: none; }
.d-block { display: block; }
.d-flex { display: flex; }
.d-grid { display: grid; }

/* Flexbox */
.flex-row { flex-direction: row; }
.flex-col { flex-direction: column; }
.items-center { align-items: center; }
.justify-center { justify-content: center; }
.justify-between { justify-content: space-between; }

/* Text */
.text-center { text-align: center; }
.text-left { text-align: left; }
.text-right { text-align: right; }
.font-bold { font-weight: 700; }
.font-medium { font-weight: 500; }

/* Spacing */
.m-0 { margin: 0; }
.p-0 { padding: 0; }
.mx-auto { margin-left: auto; margin-right: auto; }
```

## Responsive Pattern

Mobile-first responsive design:

```fscss
/* Base (mobile) */
.container {
  padding: $spacing-md;
}

.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: $spacing-md;
}

/* Tablet */
@media (min-width: 768px) {
  .container {
    padding: $spacing-lg;
  }

  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .container {
    padding: $spacing-xl;
    max-width: 1200px;
    margin: 0 auto;
  }

  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

## Animation Pattern

Consistent animations and transitions:

```fscss
/* Transition definitions */
@fun(fade-in) {
  -*transition: opacity 0.3s ease;
}

@fun(slide-up) {
  -*transition: transform 0.3s ease, opacity 0.3s ease;
}

@fun(scale) {
  -*transition: transform 0.2s ease;
}

/* Usage */
.fade-in {
  @fun.fade-in;
  opacity: 0;

  &.is-visible {
    opacity: 1;
  }
}

.slide-up {
  @fun.slide-up;
  -*transform: translateY(20px);
  opacity: 0;

  &.is-visible {
    -*transform: translateY(0);
    opacity: 1;
  }
}

.hover-scale {
  @fun.scale;

  &:hover {
    -*transform: scale(1.05);
  }
}
```

## Theme Pattern

Light and dark theme support:

```fscss
/* Light theme (default) */
:root {
  --bg-primary: #ffffff;
  --bg-secondary: #f8fafc;
  --text-primary: #0f172a;
  --text-secondary: #64748b;
  --border: #e2e8f0;
}

/* Dark theme */
[data-theme="dark"] {
  --bg-primary: #0f172a;
  --bg-secondary: #1e293b;
  --text-primary: #f8fafc;
  --text-secondary: #94a3b8;
  --border: #334155;
}

/* Usage */
body {
  background: var(--bg-primary);
  color: var(--text-primary);
}

.card {
  background: var(--bg-secondary);
  border: 1px solid var(--border);
}
```

## Exercises

1. Build a card component using BEM naming
2. Create a utility-first system with common classes
3. Implement a responsive grid pattern

---

**Next: [Imports](./imports.md)**
