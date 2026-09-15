# Shared Properties

Reuse property values across multiple selectors. Keep your CSS consistent and maintainable.

## Variables for Shared Values

The simplest way to share values:

```fscss
$primary: #2563eb;
$spacing: 1rem;
$radius: 8px;

.button {
  background: $primary;
  padding: $spacing;
  border-radius: $radius;
}

.card {
  border: 1px solid $primary;
  padding: $spacing;
  border-radius: $radius;
}
```

## @fun for Shared Patterns

Group related properties together:

```fscss
@fun(card) {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}

.product-card { @fun.card; }
.blog-card { @fun.card; }
.user-card { @fun.card; }
```

## @define for Parameterized Sharing

Pass different values to the same pattern:

```fscss
@define spacing-unit(name, value) {
  .mt-#{$name} { margin-top: $value; }
  .mb-#{$name} { margin-bottom: $value; }
  .pt-#{$name} { padding-top: $value; }
  .pb-#{$name} { padding-bottom: $value; }
}

@spacing-unit(0, 0);
@spacing-unit(1, 0.25rem);
@spacing-unit(2, 0.5rem);
@spacing-unit(3, 0.75rem);
@spacing-unit(4, 1rem);
```

## Shared Component Base

Create a base that components extend:

```fscss
/* Base styles shared by all buttons */
@fun(button-base) {
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  border: none;
  -*transition: all 0.2s ease;
}

/* Button variants using the base */
.btn-primary {
  @fun(button-base);
  background: #2563eb;
  color: white;
}

.btn-secondary {
  @fun(button-base);
  background: #64748b;
  color: white;
}

.btn-outline {
  @fun(button-base);
  background: transparent;
  color: #2563eb;
  border: 2px solid #2563eb;
}
```

## Shared Design Tokens

Define your design system once:

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
$font-family: 'Inter', system-ui, sans-serif;
$font-size-sm: 0.875rem;
$font-size-base: 1rem;
$font-size-lg: 1.125rem;

/* Borders */
$radius-sm: 4px;
$radius-md: 8px;
$radius-lg: 12px;

/* Shadows */
$shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
$shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
$shadow-lg: 0 10px 25px rgba(0, 0, 0, 0.15);
```

Use these tokens everywhere:

```fscss
.card {
  background: white;
  border-radius: $radius-md;
  box-shadow: $shadow-md;
  padding: $spacing-lg;
}

.button {
  background: $color-primary;
  color: white;
  padding: $spacing-sm $spacing-md;
  border-radius: $radius-md;
  font-size: $font-size-base;
}

.text {
  font-family: $font-family;
  font-size: $font-size-base;
  color: $color-secondary;
}
```

## Shared Mixins

Create reusable style blocks:

```fscss
@fun(flex-center) {
  display: flex;
  align-items: center;
  justify-content: center;
}

@fun(flex-between) {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

@fun(container) {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 $spacing-md;
}

/* Use them */
.page { @fun(container); }
.header { @fun(flex-between); }
.centered { @fun(flex-center); }
```

## Best Practices

### 1. Define Tokens First

```fscss
/* Good: Tokens at the top */
$primary: #2563eb;

.button {
  background: $primary;
}

/* Avoid: Inline values */
.button {
  background: #2563eb;
}
```

### 2. Use Meaningful Names

```fscss
/* Good: Clear names */
$spacing-md: 1rem;

/* Avoid: Unclear names */
$space: 1rem;
$s: 1rem;
```

### 3. Keep It Simple

```fscss
/* Good: Simple, reusable */
@fun(card) {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
}

/* Avoid: Over-engineered */
@fun(complex-card-system-with-everything) {
  /* 50 lines of code */
}
```

## Exercises

1. Create a complete design token system
2. Build shared component bases for buttons, cards, and forms
3. Design a utility system with shared properties

---

**Next: [Advanced Selectors](./advanced-selectors.md)**
