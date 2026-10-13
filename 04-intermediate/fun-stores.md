# Function Stores (`@fun`)

`@define` creates reusable *logic*. `@fun` creates reusable *data* — a named block of declarations you reference by name. It is FSCSS's native design-token store: one place your palette, spacing scale, or shadows live.

## Declaring a Store

```fscss
@fun(card-style){
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}
```

## Using the Whole Block

Write `@fun.name` where you want the declarations injected:

```fscss
.card {
  @fun.card-style
  border: 1px solid #e2e8f0;
}
```

```css
.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  border: 1px solid #e2e8f0;
}
```

## Pulling a Single Property

`@fun.name.property` injects one declaration:

```fscss
@fun(box){
  width: 100px;
  height: 200px;
}

.size-card {
  @fun.box.width
  @fun.box.height
}
```

## Pulling Just a Value

Add `.value` to get only the value, which is what you need inside a larger value like `linear-gradient(...)` or `var(...)`:

```fscss
@fun(col){
  1: #550066;
  2: #005523;
}

.box {
  background: linear-gradient(@fun.col.1.value, @fun.col.2.value);
}
```

```css
.box {
  background: linear-gradient(#550066, #005523);
}
```

## The Three Access Modes

| Reference | You get |
|-----------|---------|
| `@fun.name` | The whole block of declarations |
| `@fun.name.property` | A single `property: value;` declaration |
| `@fun.name.property.value` | Just the value |

Memorise these three and `@fun` becomes a second nature.

## Stores as Design Tokens

This is the intended use. Put your palette in a store, then publish it as CSS custom properties:

```fscss
@fun(palette){
  magenta: #ff2ecb;
  cyan:    #22d3ee;
  green:   #4ade80;
}

:root {
  --blob-magenta-bg: @fun.palette.magenta.value;
  --blob-cyan-bg:    @fun.palette.cyan.value;
  --blob-green-bg:   @fun.palette.green.value;
}
```

Now every color has three lives: one source of truth (the store), a runtime token (`--blob-*-bg`), and a clean reference point for further code.

## A Spacing Scale

```fscss
@fun(space){
  sm: 0.5rem;
  md: 1rem;
  lg: 1.5rem;
  xl: 2rem;
}

.stack {
  gap: @fun.space.md.value;
  padding: @fun.space.lg.value;
}

.section {
  margin-block: @fun.space.xl.value;
}
```

Because `.value` produces a bare value, it drops neatly into any property.

## `@fun` vs `str()` vs `@define`

| Tool | Holds | Parameters | Access |
|------|-------|-----------|--------|
| `str()` | A block of CSS text | No | Bare label |
| `@fun` | Named key/value declarations | No | Dot notation |
| `@define` | A parameterized template | Yes | `@name(args)` |

Use `str()` for a quick one-off block, `@fun` for named data, and `@define` when something needs arguments.

## A Full Example

```fscss
@fun(brand){
  primary: #2563eb;
  ink:     #0f172a;
  muted:   #64748b;
}

@fun(radius){
  sm: 6px;
  md: 10px;
  lg: 16px;
}

.cta {
  background: @fun.brand.primary.value;
  color: white;
  border-radius: @fun.radius.md.value;
}

.cta-note {
  color: @fun.brand.muted.value;
  border-radius: @fun.radius.sm.value;
}
```

## Summary

- `@fun(name){ ... }` declares a named declaration block.
- `@fun.name` injects the block; `.property` gets one declaration; `.property.value` gets the value.
- Stores are the natural home for palettes and scales.
- Combine with `--` tokens to publish data at runtime.

---

**Next: [Array Basics](./arrays-basics.md)**
