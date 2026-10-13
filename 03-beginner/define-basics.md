# Defining Blocks with `@define`

Variables reuse *values*. `@define` reuses *whole style patterns*. It is the FSCSS version of a mixin: define it once with parameters, call it wherever you need it.

## The Basic Shape

```fscss
@define name(param: default) {
  property: @use(param);
}
```

- `@define` starts the block.
- Parameters go in parentheses and can have defaults.
- Inside the body, read a parameter with `@use(param)`.
- Call the define with `@name(args)`.

## A First Define

```fscss
@define button(bg, color) {
  background: @use(bg);
  color: @use(color);
  padding: 12px 24px;
  border-radius: 8px;
}

.btn-primary {
  @button(#2563eb, white)
}

.btn-danger {
  @button(#dc2626, white)
}
```

Compiles to:

```css
.btn-primary {
  background: #2563eb;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
}
.btn-danger {
  background: #dc2626;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
}
```

## The One Rule That Bites Everyone

Parameters **must** be read with `@use(...)`. A bare reference does not resolve:

```fscss
/* Wrong — `bg` is printed literally */
@define bad(bg) {
  background: bg;
}

/* Right */
@define good(bg) {
  background: @use(bg);
}
```

If you ever see the parameter's name show up in your compiled CSS, you forgot `@use`.

## Defaults

Give a parameter a default so callers can omit it:

```fscss
@define card(bg: white, radius: 8px, pad: 1.5rem) {
  background: @use(bg);
  border-radius: @use(radius);
  padding: @use(pad);
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.card {
  @card()
}

.card-dark {
  @card(#1e293b, 12px)
}
```

`@card()` uses all three defaults. `@card(#1e293b, 12px)` overrides the first two and keeps the default padding.

## Composition

Defines are meant to be small and stacked. Build a component out of focused pieces:

```fscss
@define btn-base(pad: 0.75rem 1.5rem) {
  padding: @use(pad);
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

@define btn-primary(bg: #3b82f6) {
  background: @use(bg);
  color: white;
}

.primary-btn {
  @btn-base()
  @btn-primary()
}
```

```css
.primary-btn {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  background: #3b82f6;
  color: white;
}
```

This is the pattern real FSCSS libraries use: many tiny defines, combined at the call site.

## Mixing Defines with Plain CSS

A define call sits next to ordinary declarations without ceremony:

```fscss
@define badge(bg, fg) {
  background: @use(bg);
  color: @use(fg);
}

.tag {
  @badge(#dbeafe, #1e40af)
  font-size: 12px;
  padding: 4px 8px;
  border-radius: 999px;
}
```

## A Teaser: Block Defines

Everything above produces *declarations inside a rule*. `@define` can also produce *whole CSS structures* — media queries, keyframes, entire rules — by wrapping the body in backticks:

```fscss
@define mobile(size) {
  `
  @media (max-width: @use(size)) {
    .container { padding: 10px; }
  }
  `
}

@mobile(768px)
```

We unpack block defines fully in the Intermediate section. For now, remember the two flavours: property defines and block defines.

## Naming Tips

- Name the define for *what it produces*: `card`, `button`, `alert`.
- Keep them small. One job each.
- Prefer parameters with sensible defaults so calls stay short.

## Summary

- `@define` creates a reusable, parameterized style block.
- Read parameters with `@use(param)` — always.
- Defaults make calls terse.
- Compose components from several small defines.

---

**Next: [Runtime vs CLI](./runtime-vs-cli.md)**
