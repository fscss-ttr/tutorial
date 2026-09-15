# Style Stores

Style stores (`@fun`) let you group related CSS properties and apply them with one line. They're like reusable style blocks.

## Basic Syntax

Define a style store with `@fun`:

```fscss
@fun(card) {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}
```

Apply it with `@fun.card`:

```fscss
.product-card {
  @fun.card;
  border: 1px solid #e2e8f0;
}
```

Compiles to:

```css
.product-card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  border: 1px solid #e2e8f0;
}
```

## Why Use Style Stores?

### 1. DRY Code (Don't Repeat Yourself)

Without style stores:

```fscss
.card-1 {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}

.card-2 {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}

.card-3 {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}
```

With style stores:

```fscss
@fun(card) {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}

.card-1 { @fun.card; }
.card-2 { @fun.card; }
.card-3 { @fun.card; }
```

### 2. Easy Maintenance

Change the card style once, and all cards update:

```fscss
@fun(card) {
  background: white;
  border-radius: 8px;  /* Change this to 12px */
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}
```

### 3. Consistent Design

Ensure all components use the same base styles:

```fscss
@fun(button) {
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  -*transition: all 0.2s ease;
}

.btn-primary { @fun.button; background: #2563eb; color: white; }
.btn-secondary { @fun.button; background: #64748b; color: white; }
.btn-danger { @fun.button; background: #dc2626; color: white; }
```

## Multiple Style Stores

Define as many as you need:

```fscss
@fun(card) {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}

@fun(button) {
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}

@fun(input) {
  width: 100%;
  padding: 12px 16px;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  font-size: 16px;
}

/* Use them */
.product-card { @fun.card; }
.submit-btn { @fun.button; background: #2563eb; color: white; }
.search-input { @fun.input; }
```

## Combining with Variables

Style stores work perfectly with variables:

```fscss
$primary: #2563eb;
$shadow: 0 4px 6px rgba(0, 0, 0, 0.1);

@fun(card) {
  background: white;
  border-radius: 8px;
  box-shadow: $shadow;
  padding: 1.5rem;
}

@fun(button) {
  background: $primary;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
}

.card { @fun.card; }
.btn { @fun.button; }
```

## Practical Examples

### Alert Boxes

```fscss
@fun(alert) {
  padding: 1rem 1.5rem;
  border-radius: 8px;
  margin-bottom: 1rem;
  font-weight: 500;
}

.alert-success {
  @fun(alert);
  background: #dcfce7;
  color: #166534;
  border: 1px solid #bbf7d0;
}

.alert-warning {
  @fun(alert);
  background: #fef3c7;
  color: #92400e;
  border: 1px solid #fde68a;
}

.alert-danger {
  @fun(alert);
  background: #fee2e2;
  color: #991b1b;
  border: 1px solid #fecaca;
}
```

### Navigation

```fscss
@fun(nav-link) {
  display: inline-block;
  padding: 0.5rem 1rem;
  color: #334155;
  text-decoration: none;
  -*transition: color 0.2s ease;
}

.nav-link { @fun(nav-link); }
.nav-link:hover { color: #2563eb; }
.nav-link.active { color: #2563eb; font-weight: 600; }
```

### Form Elements

```fscss
@fun(form-group) {
  margin-bottom: 1rem;
}

@fun(form-label) {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
  color: #334155;
}

@fun(form-input) {
  width: 100%;
  padding: 12px 16px;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  font-size: 16px;
}

.form-group { @fun.form-group; }
.form-label { @fun.form-label; }
.form-input { @fun.form-input; }
```

## Naming Tips

Use descriptive names for style stores:

```fscss
/* Good */
@fun(card) { ... }
@fun(button-primary) { ... }
@fun(alert-warning) { ... }

/* Avoid */
@fun(style1) { ... }
@fun(bs) { ... }
@fun(x) { ... }
```

## When to Use Style Stores

| Use Style Stores When... | Use Variables When... |
|--------------------------|----------------------|
| Repeating a group of properties | Repeating a single value |
| Multiple components share styles | Colors, sizes, or spacing |
| Creating a design system | Creating a theme |

## Exercises

1. Create a style store for button variants
2. Build an alert system with style stores
3. Refactor existing CSS by extracting repeated patterns

## Summary

Style stores are FSCSS's answer to repetitive CSS patterns. Define once, apply everywhere. They make your code:

- **DRY** — No repetition
- **Maintainable** — Change in one place
- **Consistent** — Same styles everywhere
- **Readable** — Named patterns are clear

---

**Next: [Basic Selectors](./basic-selectors.md)**
