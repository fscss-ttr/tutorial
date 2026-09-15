# @define Blocks

`@define` creates reusable, parameterized style definitions. Think of them as functions for CSS.

## Basic Syntax

```fscss
@define button(bg, color) {
  background: @use(bg);
  color: @use(color);
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}
```

Use it:

```fscss
.btn-primary {
  @button(#2563eb, white);
}

.btn-danger {
  @button(#dc2626, white);
}
```

Compiles to:

```css
.btn-primary {
  background: #2563eb;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}

.btn-danger {
  background: #dc2626;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}
```

## @define vs @fun

| Feature | @define | @fun |
|---------|---------|------|
| Parameters | Yes | No |
| Reusability | High | Medium |
| Use case | Component variants | Shared patterns |

Use `@define` when you need to pass different values. Use `@fun` when the styles are always the same.

## Parameters

### Single Parameter

```fscss
@define text-size(size) {
  font-size: @use(size);
}

.large { @text-size(1.5rem); }
.small { @text-size(0.875rem); }
```

### Multiple Parameters

```fscss
@define spacing(top, right, bottom, left) {
  padding-top: @use(top);
  padding-right: @use(right);
  padding-bottom: @use(bottom);
  padding-left: @use(left);
}

.box { @spacing(1rem, 2rem, 1rem, 2rem); }
紧凑 { @spacing(0.5rem, 0.5rem, 0.5rem, 0.5rem); }
```

### No Parameters

```fscss
@define flex-center() {
  display: flex;
  align-items: center;
  justify-content: center;
}

.centered { @flex-center(); }
```

## Practical Examples

### Card Variants

```fscss
@define card(padding, radius, shadow) {
  background: white;
  padding: @use(padding);
  border-radius: @use(radius);
  box-shadow: @use(shadow);
}

.card-sm { @card(1rem, 8px, 0 2px 4px rgba(0,0,0,0.1)); }
.card-md { @card(1.5rem, 12px, 0 4px 6px rgba(0,0,0,0.1)); }
.card-lg { @card(2rem, 16px, 0 10px 25px rgba(0,0,0,0.15)); }
```

### Alert System

```fscss
@define alert(bg, border, text) {
  background: @use(bg);
  border: 1px solid @use(border);
  color: @use(text);
  padding: 1rem 1.5rem;
  border-radius: 8px;
  margin-bottom: 1rem;
}

.alert-info { @alert(#dbeafe, #bfdbfe, #1e40af); }
.alert-success { @alert(#dcfce7, #bbf7d0, #166534); }
.alert-warning { @alert(#fef3c7, #fde68a, #92400e); }
.alert-danger { @alert(#fee2e2, #fecaca, #991b1b); }
```

### Typography

```fscss
@define heading(level, size, weight) {
  font-size: @use(size);
  font-weight: @use(weight);
  line-height: 1.2;
  margin-bottom: 0.5rem;
  color: #0f172a;
}

h1 { @heading(1, 2.5rem, 700); }
h2 { @heading(2, 2rem, 700); }
h3 { @heading(3, 1.5rem, 600); }
h4 { @heading(4, 1.25rem, 600); }
```

### Grid System

```fscss
@define grid(columns, gap) {
  display: grid;
  grid-template-columns: repeat(@use(columns), 1fr);
  gap: @use(gap);
}

.grid-2 { @grid(2, 1rem); }
.grid-3 { @grid(3, 1.5rem); }
.grid-4 { @grid(4, 2rem); }
```

## Advanced Usage

### Nested @define

```fscss
@define button-base() {
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  border: none;
}

@define button-primary() {
  @button-base();
  background: #2563eb;
  color: white;
}

@define button-outline() {
  @button-base();
  background: transparent;
  color: #2563eb;
  border: 2px solid #2563eb;
}
```

### @define with Variables

```fscss
$primary: #2563eb;

@define themed-button(bg) {
  background: @use(bg);
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
}

.btn { @themed-button($primary); }
```

## Tips

1. **Name clearly** — Use descriptive names like `card-variant`, not `cv`
2. **Keep it focused** — One @define, one purpose
3. **Use sensible defaults** — Consider what most users will need
4. **Document complex ones** — Add comments for advanced @define blocks

## Exercises

1. Create a @define for different border styles
2. Build a responsive typography system with @define
3. Create a button system with size variants

---

**Next: [@fun Functions](./@fun-functions.md)**
