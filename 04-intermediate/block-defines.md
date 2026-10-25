# Block Defines

So far every `@define` produced declarations *inside* a rule. But FSCSS can also emit whole CSS structures — a media query, a keyframe animation, an entire set of rules — from a single define. You mark the body as a block by wrapping it in backticks.

## The Syntax

```css
@define mobile(size) {
  `
  @media (max-width: @use(size)) {
    .container {
      padding: 10px;
    }
  }
  `
}

@mobile(768px)
```

Parameters still use `@use(...)` — inside the backticks, same as before. The backticks tell FSCSS "this is raw CSS structure, not a list of declarations".

Compiles to:

```css
@media (max-width: 768px) {
  .container {
    padding: 10px;
  }
}
```

## Generating Media Queries

A breakpoint helper is the classic block define:

```css
@define upto(breakpoint) {
  `
  @media (max-width: @use(breakpoint)) {
    @use(breakpoint-body)
  }
  `
}
```

In practice you'll write a define per breakpoint, or pass the whole body in from the call site using a selector parameter (shown below):

```css
@define tablet() {
  `
  @media (min-width: 768px) {
    .grid { grid-template-columns: repeat(2, 1fr); }
  }
  `
}

.tablet-only { display: none; }

@tablet()
```

## Generating Keyframes

```css
@define spin(duration: 1s) {
  `
  @keyframes spin {
    from { transform: rotate(0deg); }
    to   { transform: rotate(360deg); }
  }
  .spinner {
    animation: spin @use(duration) linear infinite;
  }
  `
}

@spin(1.4s)
```

One call, both the `@keyframes` and the rule that uses it.

## Selector Parameters

A block define can take a selector as a parameter and scope its output to it:

```css
@define siri-wave(st: .siri-wave) {
  `
  @use(st) {
    position: relative;
    ov: hidden;
    rounded: 24px;
    bg: #000;
  }
  `
}

.siri-wave { @siri-wave() }
```

`@use(st) { ... }` emits a rule whose selector is the parameter. Add `*` for a universal descendant:

```css
@define reset-all(st: .siri-wave) {
  `
  @use(st), @use(st) * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  `
}
```

Compiles to `.siri-wave, .siri-wave * { ... }`.

## A Real Example: Responsive Container

```css
@define container() {
  `
  .container {
    width: 100%;
    margin: 0 auto;
    padding: 0 1rem;
  }
  @media (min-width: 768px) {
    .container { max-width: 720px; padding: 0 2rem; }
  }
  @media (min-width: 1024px) {
    .container { max-width: 960px; }
  }
  `
}

@container()
```

## Property Defines vs Block Defines

| | Property define | Block define |
|---|---|---|
| Body | Plain declarations | Backtick string |
| Output | Declarations inside the calling rule | Whole CSS structures |
| Use for | Components, resets | Media queries, keyframes, multi-rule output |
| Has a call-site rule? | Yes, you call it inside one | No, it stands alone |

If your define both sets properties on the caller *and* emits top-level rules, split it into two defines: one property define, one block define. Keep them honest.

## Calling a Block Define

Block defines are called like any other define, as a statement:

```css
@tablet()
@spin(2s)
@container()
```

No trailing semicolon required.

## Summary

- Wrap a define body in backticks to emit full CSS structures.
- Use selector parameters (`@use(st)`, `@use(st) *`) to scope a block define.
- Perfect for media queries, keyframes, and multi-rule output.
- Split a mixed define into a property define plus a block define.

---

**Next: [Function Stores](./fun-stores.md)**
