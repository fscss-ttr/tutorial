# Variables

Variables are the foundation of FSCSS. They let you store values once and reuse them everywhere.

## Basic Syntax

Define a variable with a dollar sign (`$`):

```fscss
$primary: #2563eb;
```

Use it anywhere in your styles:

```fscss
$primary: #2563eb;

.button {
  background: $primary;
}
```

Compiles to:

```css
.button {
  background: #2563eb;
}
```

## Why Use Variables?

### 1. Change Values Once, Update Everywhere

Without variables:

```fscss
.header { background: #2563eb; }
.button { background: #2563eb; }
.link { color: #2563eb; }
```

To change the color, you'd need to update three places.

With variables:

```fscss
$primary: #2563eb;

.header { background: $primary; }
.button { background: $primary; }
.link { color: $primary; }
```

Change `$primary` once, and everything updates.

### 2. Self-Documenting Code

```fscss
$font-size-base: 16px;
$spacing-large: 2rem;
$color-text: #334155;
$border-radius: 8px;

body {
  font-size: $font-size-base;
  color: $color-text;
}

.container {
  padding: $spacing-large;
  border-radius: $border-radius;
}
```

The variable names tell you what each value means.

### 3. Consistent Design System

```fscss
/* Colors */
$color-primary: #2563eb;
$color-secondary: #64748b;
$color-success: #22c55e;
$color-danger: #dc2626;

/* Spacing */
$spacing-xs: 0.25rem;
$spacing-sm: 0.5rem;
$spacing-md: 1rem;
$spacing-lg: 1.5rem;
$spacing-xl: 2rem;

/* Typography */
$font-size-sm: 0.875rem;
$font-size-base: 1rem;
$font-size-lg: 1.125rem;
$font-size-xl: 1.25rem;

/* Borders */
$radius-sm: 4px;
$radius-md: 8px;
$radius-lg: 12px;
$radius-full: 9999px;
```

## Variable Types

FSCSS variables work with all CSS value types:

### Colors

```fscss
$primary: #2563eb;
$secondary: rgb(100, 116, 139);
$success: hsl(142, 76%, 36%);
$transparent: rgba(0, 0, 0, 0.5);
```

### Spacing

```fscss
$small: 8px;
$medium: 1rem;
$large: 1.5rem;
$huge: 2rem;
```

### Typography

```fscss
$font-family: 'Inter', sans-serif;
$font-size: 16px;
$font-weight: 600;
$line-height: 1.6;
```

### Other Values

```fscss
$border: 1px solid #e2e8f0;
$shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
$transition: all 0.3s ease;
$z-index: 100;
```

## Naming Conventions

Use clear, consistent names:

### Kebab Case (Recommended)

```fscss
$primary-color: #2563eb;
$font-size-base: 16px;
$spacing-large: 2rem;
```

### Avoid

```fscss
$primaryColor: #2563eb;  /* Mixed case */
$fontsizebase: 16px;     /* No separators */
$font_size_base: 16px;   /* Snake case (works but not recommended) */
```

## Scoping

Variables in FSCSS follow CSS scoping rules:

```fscss
$global: blue;

.container {
  $local: red;

  .button {
    color: $global;  /* Works */
    background: $local;  /* May not work depending on scope */
  }
}
```

## Practical Examples

### Theme Colors

```fscss
/* Theme */
$bg-primary: #ffffff;
$bg-secondary: #f8fafc;
$text-primary: #0f172a;
$text-secondary: #64748b;

body {
  background: $bg-primary;
  color: $text-primary;
}

.card {
  background: $bg-secondary;
  color: $text-secondary;
}
```

### Responsive Spacing

```fscss
$gap-mobile: 1rem;
$gap-tablet: 1.5rem;
$gap-desktop: 2rem;

.container {
  padding: $gap-mobile;
}

@media (min-width: 768px) {
  .container {
    padding: $gap-tablet;
  }
}

@media (min-width: 1024px) {
  .container {
    padding: $gap-desktop;
  }
}
```

### Component Styles

```fscss
$btn-padding: 12px 24px;
$btn-radius: 8px;
$btn-font: 600 14px/1.5 system-ui;

.button {
  padding: $btn-padding;
  border-radius: $btn-radius;
  font: $btn-font;
  border: none;
  cursor: pointer;
}
```

## Exercises

1. Create a color palette using variables
2. Define spacing variables for a design system
3. Refactor existing CSS by extracting repeated values into variables

## Summary

Variables are simple but powerful. They make your CSS:

- **Easier to maintain** — Change values in one place
- **More consistent** — Use the same values everywhere
- **Self-documenting** — Variable names explain their purpose
- **Organized** — Group related values together

Master variables before moving on — they're used in every FSCSS file you'll write.

---

**Next: [Shorthand Syntax](./shorthand-syntax.md)**
