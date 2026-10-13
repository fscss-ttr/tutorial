# 01 — Button System

Every design system starts here. Build a set of buttons — solid, outline, ghost, and a disabled state — driven by tokens and defines so adding a variant is one line.

## The Brief

- A `btn` family with `primary`, `secondary`, and `outline` variants.
- Consistent padding, radius, and font from tokens.
- A hover state and a sibling `:disabled` state per variant.
- A `size` modifier (`btn-sm`, `btn-lg`).

## The FSCSS

```fscss
/* tokens */
@fun(color){
  primary: #2563eb;
  primary-strong: #1d4ed8;
  ink: #0f172a;
  white: #ffffff;
}

@fun(space){
  sm: 0.5rem 1rem;
  md: 0.75rem 1.5rem;
  lg: 1rem 2rem;
}

@define btn-base() {
  border: none;
  border-radius: 8px;
  font-family: inherit;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.15s ease, color 0.15s ease,
              box-shadow 0.15s ease;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

@define btn-solid(bg: #2563eb, fg: #ffffff) {
  @btn-base()
  background: @use(bg);
  color: @use(fg);
}

.btn-primary {
  @btn-solid(@fun.color.primary.value, @fun.color.white.value)
}

.btn-primary:hover {
  background: @fun.color.primary-strong.value;
}

.btn-primary:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-outline {
  @btn-base()
  background: transparent;
  color: @fun.color.primary.value;
  border: 2px solid @fun.color.primary.value;
}

.btn-outline:hover {
  background: rgba(37, 99, 235, 0.08);
}

.btn-sm { padding: @fun.space.sm.value; }
.btn-md { padding: @fun.space.md.value; }
.btn-lg { padding: @fun.space.lg.value; }
```

## The HTML

```html
<button class="btn-primary btn-md">Primary</button>
<button class="btn-primary btn-sm">Small</button>
<button class="btn-outline btn-md">Outline</button>
<button class="btn-primary btn-md" disabled>Disabled</button>
```

## What Happens

- Every button shares `btn-base` — one place to change *all* padding/radius.
- `@fun.color`/`@fun.space` are the single source for palette and spacing.
- New variants are a 4-line define + one class (stretch goal: add `btn-ghost`).
- `:disabled` needs its own hover-proof rule because hover background still fires.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a `btn-ghost` variant (text-only, hover tint).
2. Make the base size depend on a `.btn-lg` inline override using `%2` on padding.
3. Add a focus-visible ring shared by all variants.

## Summary

Tokens → base define → variant defines → hover/disabled state. That ordering is the entire button system, and it scales to fifty variants without duplicating a padding line.

---

**Next: [02 — Card Component](../02-card-component.md)**