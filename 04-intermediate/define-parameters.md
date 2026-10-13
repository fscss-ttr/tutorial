# Defining with Parameters

You met `@define` in the beginner section. Now let's use it properly: multiple parameters, defaults that pull their weight, and composition that keeps every block small.

## The Contract

```fscss
@define name(param1, param2: default) {
  property: @use(param1);
  property: @use(param2);
}
```

Two rules never change:

1. Parameters are read with `@use(param)`.
2. Call with `@name(value1, value2)`.

Everything else is style.

## Multiple Parameters

```fscss
@define button(bg, color, pad: 12px 24px, radius: 8px) {
  background: @use(bg);
  color: @use(color);
  padding: @use(pad);
  border-radius: @use(radius);
  border: none;
  cursor: pointer;
}
```

```fscss
.btn-primary { @button(#2563eb, white) }
.btn-ghost   { @button(transparent, #2563eb, 8px 16px) }
```

Required parameters come first; anything optional gets a default and comes after. That keeps calls readable.

## Defaults That Earn Their Keep

A good default is the value you would have typed anyway. Compare:

```fscss
/* Annoying — every call repeats the obvious */
@define card(bg, radius, pad, shadow) { /* ... */ }
.card { @card(white, 8px, 1.5rem, 0 4px 6px rgba(0,0,0,.1)) }

/* Better — the common case is free */
@define card(bg: white, radius: 8px, pad: 1.5rem, shadow: 0 4px 6px rgba(0,0,0,.1)) { /* ... */ }
.card        { @card() }
.card-dark   { @card(#1e293b, 12px) }
```

## Parameters Feed Everything

Parameters are just text. They can appear inside values, selectors, even other `@define` calls:

```fscss
@define spacing(prop, size) {
  @use(prop): @use(size);
}
```

Wait — you cannot use a parameter as a property name in a property define. Property names are literal. Parameters belong in *values*:

```fscss
@define inset(amount) {
  top: @use(amount);
  right: @use(amount);
  bottom: @use(amount);
  left: @use(amount);
}

.overlay {
  position: absolute;
  @inset(0)
}
```

If you need the property name to vary, that is a signal to use a block define (next lesson) or simply write two defines.

## Composition: Small Blocks, Stacked

The real power move is building components from focused, single-purpose defines:

```fscss
@define flex-center() {
  display: flex;
  align-items: center;
  justify-content: center;
}

@define surface(bg: white, radius: 12px) {
  background: @use(bg);
  border-radius: @use(radius);
}

@define elevate(level: 1) {
  box-shadow: 0 calc(4px * 1px) calc(6px * 1px) rgba(0,0,0,.1);
}

.panel {
  @flex-center()
  @surface()
  @elevate()
  width: 240px;
  height: 160px;
}
```

Each define does one thing. Compose at the call site. This is how maintainable FSCSS is written.

## Using Variables Inside Defines

Defines can read global `$` variables, which lets a theme flow through every component:

```fscss
$brand: #7c3aed;
$brand-ink: white;

@define primary-button() {
  background: $brand!;
  color: $brand-ink!;
  padding: 12px 24px;
  border: none;
  border-radius: 8px;
}

.cta { @primary-button() }
```

Change `$brand` once and every `@primary-button()` call follows.

## Nesting One Define in Another

A define body may call another define:

```fscss
@define btn-base() {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

@define btn-primary(bg: #3b82f6) {
  @btn-base()
  background: @use(bg);
  color: white;
}

.submit { @btn-primary(#10b981) }
```

```css
.submit {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  background: #10b981;
  color: white;
}
```

## Pitfalls

```fscss
/* Forgot @use — prints the name literally */
@define bad(bg) { background: bg; }

/* Defaults before required params — confusing to call */
@define shuffle(bg: white, color) { /* ... */ } @shuffle(white, red)

/* One mega-define instead of three small ones */
@define everything(a,b,c,d,e,f,g,h) { /* ... */ }
```

Keep parameters ordered (required, then optional), keep defines small, and always `@use`.

## Summary

- Parameters are read with `@use(param)` and passed positionally at the call.
- Give common values defaults so the average call stays short.
- Compose components from several small defines.
- Defines can read global `$` variables, so themes flow through.

---

**Next: [Block Defines](./block-defines.md)**
