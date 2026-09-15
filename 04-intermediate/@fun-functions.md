# @fun Functions

`@fun` creates reusable style blocks without parameters. Apply the same set of properties to multiple selectors.

## Basic Syntax

```fscss
@fun(card) {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}
```

Apply it:

```fscss
.product-card { @fun.card; }
.blog-card { @fun.card; }
.user-card { @fun.card; }
```

## @fun vs @define

| Use @fun when... | Use @define when... |
|-------------------|---------------------|
| Styles are always the same | Styles change per use |
| No parameters needed | Parameters needed |
| Simple reuse | Component variants |

## Multiple @fun Blocks

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
```

Use them:

```fscss
.card { @fun.card; }
.btn { @fun.button; background: #2563eb; color: white; }
.field { @fun.input; }
```

## Combining @fun with Variables

```fscss
$primary: #2563eb;
$shadow: 0 4px 6px rgba(0, 0, 0, 0.1);

@fun(card) {
  background: white;
  border-radius: 8px;
  box-shadow: $shadow;
  padding: 1.5rem;
}

@fun(primary-button) {
  background: $primary;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
}

.card { @fun.card; }
.btn { @fun.primary-button; }
```

## Practical Examples

### Alert Styles

```fscss
@fun(alert-base) {
  padding: 1rem 1.5rem;
  border-radius: 8px;
  margin-bottom: 1rem;
  font-weight: 500;
}

.success { @fun.alert-base; background: #dcfce7; color: #166534; }
.warning { @fun.alert-base; background: #fef3c7; color: #92400e; }
.danger { @fun.alert-base; background: #fee2e2; color: #991b1b; }
```

### Navigation

```fscss
@fun(nav-item) {
  display: inline-block;
  padding: 0.5rem 1rem;
  color: #334155;
  text-decoration: none;
}

@fun(nav-item-hover) {
  color: #2563eb;
}

.link { @fun.nav-item; }
.link:hover { @fun.nav-item-hover; }
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
  -*transition: border-color 0.2s ease;
}

.group { @fun.form-group; }
.label { @fun.form-label; }
.input { @fun.form-input; }
.input:focus { border-color: #2563eb; outline: none; }
```

### Layout Utilities

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
  padding: 0 1rem;
}

.centered { @fun.flex-center; }
.spaced { @fun.flex-between; }
.wrap { @fun.container; }
```

## Advanced Patterns

### Chaining @fun

```fscss
@fun(card) {
  background: white;
  border-radius: 8px;
  padding: 1.5rem;
}

@fun(shadow) {
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.card { @fun.card; @fun.shadow; }
.card-elevated { @fun.card; @fun.shadow; box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15); }
```

### @fun with Selectors

```fscss
@fun(link) {
  color: #2563eb;
  text-decoration: none;
  -*transition: color 0.2s ease;
}

@fun(link-hover) {
  color: #1d4ed8;
  text-decoration: underline;
}

.nav-link {
  @fun.link;

  &:hover {
    @fun.link-hover;
  }
}
```

## Tips

1. **Group related properties** — Put margin, padding, or typography together
2. **Use meaningful names** — `@fun(card)` not `@fun(c)`
3. **Keep it focused** — One @fun, one purpose
4. **Combine with variables** — Make @fun blocks configurable

## When to Use @fun

| Scenario | Use @fun? |
|----------|-----------|
| Same styles on multiple elements | Yes |
| Different styles per element | Use @define |
| Reusable component base | Yes |
| Variant with different values | Use @define |

## Exercises

1. Create @fun blocks for a complete form (group, label, input, button)
2. Build a card system with @fun for base and shadow
3. Create navigation utilities with @fun

---

**Next: [Arrays & Loops](./arrays-loops.md)**
