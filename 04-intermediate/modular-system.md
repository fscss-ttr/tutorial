# Modular System

Organize your FSCSS code into logical modules. Keep your stylesheets maintainable as your project grows.

## Why Modularize?

Without modules:

```fscss
/* One giant file with 1000+ lines */
$primary: #2563eb;
$secondary: #64748b;
/* ... 50 more variables ... */

@fun(card) { ... }
@fun(button) { ... }
@fun(input) { ... }
/* ... 20 more @fun blocks ... */

.header { ... }
.nav { ... }
.hero { ... }
/* ... 100 more rules ... */
```

With modules:

```
styles/
├── variables.fscss
├── reset.fscss
├── typography.fscss
├── layout.fscss
├── components/
│   ├── card.fscss
│   ├── button.fscss
│   └── input.fscss
└── main.fscss
```

## Module Structure

### Recommended Directory Layout

```
src/
└── styles/
    ├── base/
    │   ├── variables.fscss
    │   ├── reset.fscss
    │   └── typography.fscss
    ├── components/
    │   ├── card.fscss
    │   ├── button.fscss
    │   ├── input.fscss
    │   └── modal.fscss
    ├── layout/
    │   ├── header.fscss
    │   ├── footer.fscss
    │   └── grid.fscss
    ├── utilities/
    │   ├── spacing.fscss
    │   ├── colors.fscss
    │   └── display.fscss
    └── main.fscss
```

### Variables Module

```fscss
/* base/variables.fscss */

/* Colors */
$primary: #2563eb;
$secondary: #64748b;
$success: #22c55e;
$warning: #f59e0b;
$danger: #dc2626;

/* Spacing */
$spacing-unit: 0.25rem;
$spacing-sm: $spacing-unit * 2;
$spacing-md: $spacing-unit * 4;
$spacing-lg: $spacing-unit * 6;
$spacing-xl: $spacing-unit * 8;

/* Typography */
$font-family: 'Inter', system-ui, sans-serif;
$font-size-sm: 0.875rem;
$font-size-base: 1rem;
$font-size-lg: 1.125rem;
$font-size-xl: 1.25rem;

/* Borders */
$radius-sm: 4px;
$radius-md: 8px;
$radius-lg: 12px;
$radius-full: 9999px;

/* Shadows */
$shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
$shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
$shadow-lg: 0 10px 25px rgba(0, 0, 0, 0.15);
```

### Component Module

```fscss
/* components/card.fscss */

@fun(card) {
  background: white;
  border-radius: $radius-md;
  box-shadow: $shadow-md;
  padding: $spacing-lg;
}

@fun(card-hover) {
  box-shadow: $shadow-lg;
  -*transform: translateY(-2px);
}

.card { @fun.card; }
.card:hover { @fun.card-hover; }
```

### Utility Module

```fscss
/* utilities/spacing.fscss */

@for $i from 0 to 6 {
  .mt-#{$i} { margin-top: $i * $spacing-unit; }
  .mb-#{$i} { margin-bottom: $i * $spacing-unit; }
  .pt-#{$i} { padding-top: $i * $spacing-unit; }
  .pb-#{$i} { padding-bottom: $i * $spacing-unit; }
}
```

## Main Entry Point

Combine all modules in your main file:

```fscss
/* main.fscss */

/* Base */
@import 'base/variables';
@import 'base/reset';
@import 'base/typography';

/* Components */
@import 'components/card';
@import 'components/button';
@import 'components/input';
@import 'components/modal';

/* Layout */
@import 'layout/header';
@import 'layout/footer';
@import 'layout/grid';

/* Utilities */
@import 'utilities/spacing';
@import 'utilities/colors';
@import 'utilities/display';
```

## Importing Modules

FSCSS supports imports:

```fscss
@import 'variables';
@import 'components/card';
@import 'components/button';
```

### Import Paths

Relative paths:

```fscss
@import '../base/variables';
@import './components/card';
```

## Module Communication

Modules can share variables:

```fscss
/* variables.fscss defines $primary */

/* button.fscss uses $primary */
.btn-primary {
  background: $primary;
  color: white;
}

/* link.fscss uses $primary */
a {
  color: $primary;
}
```

## Best Practices

### 1. One Responsibility Per Module

```fscss
/* Good: card.fscss handles only card styles */
.card { ... }
.card-header { ... }
.card-body { ... }
.card-footer { ... }

/* Bad: card.fscss handles buttons too */
.card { ... }
.btn { ... }
.btn-primary { ... }
```

### 2. Consistent Naming

```
components/
├── card.fscss
├── button.fscss
├── input.fscss
└── modal.fscss  (not modal-box.fscss or Modal.fscss)
```

### 3. Variables First

Always import variables before using them:

```fscss
@import 'variables';  /* First */
@import 'components/button';  /* Uses $primary */
```

### 4. Avoid Circular Imports

Don't import files that import each other:

```fscss
/* Good */
a.fscss → b.fscss → c.fscss

/* Bad */
a.fscss → b.fscss → a.fscss (circular!)
```

## Practical Example

### Complete Component Module

```fscss
/* components/alert.fscss */

/* Alert base styles */
@fun(alert-base) {
  padding: $spacing-md $spacing-lg;
  border-radius: $radius-md;
  margin-bottom: $spacing-md;
  font-weight: 500;
}

/* Alert variants */
@define alert-variant(bg, border, text) {
  @fun(alert-base);
  background: @use(bg);
  border: 1px solid @use(border);
  color: @use(text);
}

.alert-info { @alert-variant(#dbeafe, #bfdbfe, #1e40af); }
.alert-success { @alert-variant(#dcfce7, #bbf7d0, #166534); }
.alert-warning { @alert-variant(#fef3c7, #fde68a, #92400e); }
.alert-danger { @alert-variant(#fee2e2, #fecaca, #991b1b); }
```

## Exercises

1. Create a modular structure for a blog project
2. Build a button module with size variants
3. Create a utility module for common classes

---

**Next: [Patterns](./patterns.md)**
