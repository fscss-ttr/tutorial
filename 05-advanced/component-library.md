# Component Library

Build a complete, reusable component library with FSCSS.

## Library Structure

```
fscss-library/
├── src/
│   ├── abstracts/
│   │   ├── variables.fscss
│   │   ├── mixins.fscss
│   │   └── functions.fscss
│   ├── base/
│   │   ├── reset.fscss
│   │   ├── typography.fscss
│   │   └── elements.fscss
│   ├── components/
│   │   ├── button/
│   │   │   ├── button.fscss
│   │   │   └── button-group.fscss
│   │   ├── card/
│   │   │   └── card.fscss
│   │   ├── input/
│   │   │   ├── input.fscss
│   │   │   └── textarea.fscss
│   │   ├── alert/
│   │   │   └── alert.fscss
│   │   └── modal/
│   │       └── modal.fscss
│   ├── layout/
│   │   ├── container.fscss
│   │   ├── grid.fscss
│   │   └── flex.fscss
│   ├── utilities/
│   │   ├── spacing.fscss
│   │   ├── display.fscss
│   │   └── typography.fscss
│   └── index.fscss
├── dist/
│   └── styles.css
├── package.json
└── README.md
```

## Variables

```fscss
/* src/abstracts/variables.fscss */

/* Colors */
$primary: #2563eb;
$secondary: #64748b;
$success: #22c55e;
$warning: #f59e0b;
$danger: #dc2626;

/* Neutrals */
$white: #ffffff;
$gray-50: #f8fafc;
$gray-100: #f1f5f9;
$gray-200: #e2e8f0;
$gray-300: #cbd5e1;
$gray-400: #94a3b8;
$gray-500: #64748b;
$gray-600: #475569;
$gray-700: #334155;
$gray-800: #1e293b;
$gray-900: #0f172a;

/* Spacing */
$spacing-xs: 0.25rem;
$spacing-sm: 0.5rem;
$spacing-md: 1rem;
$spacing-lg: 1.5rem;
$spacing-xl: 2rem;
$spacing-2xl: 3rem;

/* Typography */
$font-family: 'Inter', system-ui, sans-serif;
$font-size-xs: 0.75rem;
$font-size-sm: 0.875rem;
$font-size-base: 1rem;
$font-size-lg: 1.125rem;
$font-size-xl: 1.25rem;
$font-size-2xl: 1.5rem;
$font-size-3xl: 1.875rem;

/* Borders */
$radius-sm: 4px;
$radius-md: 8px;
$radius-lg: 12px;
$radius-full: 9999px;

/* Shadows */
$shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
$shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
$shadow-lg: 0 10px 25px rgba(0, 0, 0, 0.15);

/* Transitions */
$transition-fast: 0.15s ease;
$transition-base: 0.2s ease;
$transition-slow: 0.3s ease;
```

## Button Component

```fscss
/* src/components/button/button.fscss */

/* Button base */
@fun(button-base) {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 12px 24px;
  border-radius: $radius-md;
  font-weight: 600;
  font-size: $font-size-base;
  line-height: 1.5;
  cursor: pointer;
  border: none;
  -*transition: all $transition-base;
}

/* Button variants */
@define button-variant(variant) {
  @if $variant == primary {
    background: $primary;
    color: $white;
  } @else if $variant == secondary {
    background: $secondary;
    color: $white;
  } @else if $variant == success {
    background: $success;
    color: $white;
  } @else if $variant == danger {
    background: $danger;
    color: $white;
  } @else if $variant == outline {
    background: transparent;
    border: 2px solid $primary;
    color: $primary;
  }

  &:hover {
    filter: brightness(0.9);
  }

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    filter: none;
  }
}

/* Button sizes */
@define button-size(size) {
  @if $size == sm {
    padding: 8px 16px;
    font-size: $font-size-sm;
  } @else if $size == md {
    padding: 12px 24px;
    font-size: $font-size-base;
  } @else if $size == lg {
    padding: 16px 32px;
    font-size: $font-size-lg;
  }
}

/* Apply button styles */
.btn { @fun(button-base); @button-variant(primary); }
.btn-secondary { @button-variant(secondary); }
.btn-success { @button-variant(success); }
.btn-danger { @button-variant(danger); }
.btn-outline { @button-variant(outline); }

.btn-sm { @button-size(sm); }
.btn-md { @button-size(md); }
.btn-lg { @button-size(lg); }
```

## Card Component

```fscss
/* src/components/card/card.fscss */

@fun(card) {
  background: $white;
  border-radius: $radius-lg;
  box-shadow: $shadow-md;
  overflow: hidden;
  -*transition: transform $transition-slow, box-shadow $transition-slow;
}

@fun(card-hover) {
  &:hover {
    -*transform: translateY(-4px);
    box-shadow: $shadow-lg;
  }
}

@fun(card-header) {
  padding: $spacing-lg;
  border-bottom: 1px solid $gray-200;
}

@fun(card-body) {
  padding: $spacing-lg;
}

@fun(card-footer) {
  padding: $spacing-lg;
  border-top: 1px solid $gray-200;
  background: $gray-50;
}

@fun(card-title) {
  font-size: $font-size-xl;
  font-weight: 600;
  color: $gray-900;
  margin: 0;
}

@fun(card-text) {
  color: $gray-600;
  line-height: 1.6;
  margin: 0;
}

/* Apply card styles */
.card { @fun(card); @fun(card-hover); }
.card-header { @fun(card-header); }
.card-body { @fun(card-body); }
.card-footer { @fun(card-footer); }
.card-title { @fun(card-title); }
.card-text { @fun(card-text); }
```

## Alert Component

```fscss
/* src/components/alert/alert.fscss */

@define alert(variant) {
  padding: $spacing-md $spacing-lg;
  border-radius: $radius-md;
  font-weight: 500;

  @if $variant == info {
    background: #dbeafe;
    color: #1e40af;
    border: 1px solid #bfdbfe;
  } @else if $variant == success {
    background: #dcfce7;
    color: #166534;
    border: 1px solid #bbf7d0;
  } @else if $variant == warning {
    background: #fef3c7;
    color: #92400e;
    border: 1px solid #fde68a;
  } @else if $variant == danger {
    background: #fee2e2;
    color: #991b1b;
    border: 1px solid #fecaca;
  }
}

.alert { @alert(info); }
.alert-success { @alert(success); }
.alert-warning { @alert(warning); }
.alert-danger { @alert(danger); }
```

## Main Entry Point

```fscss
/* src/index.fscss */

/* Abstracts */
@import 'abstracts/variables';
@import 'abstracts/mixins';

/* Base */
@import 'base/reset';
@import 'base/typography';

/* Components */
@import 'components/button/button';
@import 'components/card/card';
@import 'components/alert/alert';

/* Layout */
@import 'layout/container';
@import 'layout/grid';
@import 'layout/flex';

/* Utilities */
@import 'utilities/spacing';
@import 'utilities/display';
```

## Usage

```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="dist/styles.css">
</head>
<body>
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h2 class="card-title">Card Title</h2>
      </div>
      <div class="card-body">
        <p class="card-text">Card content goes here.</p>
      </div>
      <div class="card-footer">
        <button class="btn btn-primary">Action</button>
      </div>
    </div>

    <div class="alert alert-success">
      Success message!
    </div>
  </div>
</body>
</html>
```

## Building the Library

```bash
# Compile all files
fscss src/index.fscss dist/styles.css

# Watch for changes
fscss --watch src/index.fscss dist/styles.css
```

## Best Practices

### 1. Document Components

```markdown
## Button

### Variants
- `.btn-primary`
- `.btn-secondary`
- `.btn-success`
- `.btn-danger`
- `.btn-outline`

### Sizes
- `.btn-sm`
- `.btn-md`
- `.btn-lg`
```

### 2. Test Across Browsers

Test your library in Chrome, Firefox, Safari, and Edge.

### 3. Version Your Library

```json
{
  "version": "1.0.0"
}
```

## Exercises

1. Build a button component with variants
2. Create a card component with slots
3. Design an alert system with different types

---

**Next: [Testing FSCSS](./testing-fscss.md)**
