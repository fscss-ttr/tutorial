# Shorthand Syntax

FSCSS introduces shorthand syntax that reduces the amount of code you write. Learn the shortcuts that make CSS faster.

## What is Shorthand?

Shorthand means writing less code to achieve the same result. Instead of typing full property names, you use shorter alternatives that compile to standard CSS.

## Wildcard Vendor Prefixing

The biggest time-saver in FSCSS. Instead of writing multiple vendor prefixes, use `-*`:

### Before (Plain CSS)

```css
.button {
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  transform: rotate(45deg);
}
```

### After (FSCSS)

```fscss
.button {
  -*transform: rotate(45deg);
}
```

Both produce identical output.

### Common Uses

```fscss
/* Flexbox */
.container {
  -*display: flex;
  -*align-items: center;
  -*justify-content: center;
}

/* Grid */
.grid {
  -*display: grid;
  -*grid-template-columns: repeat(3, 1fr);
}

/* Transitions */
.button {
  -*transition: all 0.3s ease;
}

/* User select */
.text {
  -*user-select: none;
}
```

## Why Vendor Prefixes Matter

Different browsers sometimes need different prefixes:

| Prefix | Browser |
|--------|---------|
| `-webkit-` | Chrome, Safari, newer Opera |
| `-moz-` | Firefox |
| `-ms-` | Internet Explorer, Edge |
| `-o-` | Older Opera |

The `-*` prefix automatically adds all four, plus the standard property.

## Multiple Prefixes at Once

Apply prefixes to multiple properties:

```fscss
.hero {
  -*display: flex;
  -*flex-direction: column;
  -*align-items: center;
  -*justify-content: center;
  -*transform: translateX(-50%);
}
```

Compiles to:

```css
.hero {
  -webkit-display: flex;
  -moz-display: flex;
  -ms-display: flex;
  -o-display: flex;
  display: flex;
  -webkit-flex-direction: column;
  -moz-flex-direction: column;
  -ms-flex-direction: column;
  -o-flex-direction: column;
  flex-direction: column;
  /* ... more prefixes ... */
}
```

## Shorthand Values

FSCSS supports CSS shorthand values:

```fscss
/* Margin shorthand */
.box {
  margin: 1rem 2rem;  /* top/bottom left/right */
}

/* Padding shorthand */
.container {
  padding: 1rem 2rem 1.5rem 3rem;  /* top right bottom left */
}

/* Background shorthand */
.header {
  background: #2563eb url('bg.jpg') no-repeat center;
}

/* Font shorthand */
.text {
  font: 600 16px/1.5 'Inter', sans-serif;
}
```

## Property Grouping

Group related properties logically:

```fscss
.button {
  /* Box model */
  padding: 12px 24px;
  margin: 0;
  border: none;
  border-radius: 8px;

  /* Typography */
  font-size: 14px;
  font-weight: 600;
  line-height: 1.5;

  /* Visual */
  background: #2563eb;
  color: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

  /* Interactive */
  cursor: pointer;
  -*transition: all 0.2s ease;
}
```

## Practical Examples

### Card Component

```fscss
.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  margin-bottom: 1rem;
  -*transition: transform 0.2s ease;
}

.card:hover {
  -*transform: translateY(-2px);
  box-shadow: 0 8px 12px rgba(0, 0, 0, 0.15);
}
```

### Navigation Bar

```fscss
.nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem 2rem;
  background: white;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.nav-link {
  padding: 0.5rem 1rem;
  color: #334155;
  text-decoration: none;
  -*transition: color 0.2s ease;
}

.nav-link:hover {
  color: #2563eb;
}
```

### Form Input

```fscss
.input {
  width: 100%;
  padding: 12px 16px;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  font-size: 16px;
  line-height: 1.5;
  background: white;
  -*transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.input:focus {
  outline: none;
  border-color: #2563eb;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}
```

## Tips

1. **Use `-*` for modern properties** — Flexbox, Grid, transforms, transitions
2. **Don't use `-*` for basic properties** — `color`, `font-size`, `margin` don't need prefixes
3. **Keep it readable** — Group related properties together
4. **Comment your code** — Explain why you're using shorthand

## Exercises

1. Convert CSS with vendor prefixes to FSCSS shorthand
2. Rewrite a card component using FSCSS shorthand
3. Create a button with multiple prefixed properties

## Summary

Shorthand syntax is FSCSS's superpower. The `-*` prefix saves you from typing the same property four times. Combine this with CSS shorthand values, and you'll write CSS significantly faster.

---

**Next: [Style Stores](./style-stores.md)**
