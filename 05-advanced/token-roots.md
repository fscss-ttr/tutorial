# Token Roots (`*-root`)

The `*-root` pattern is a small convention with big payoff: define a block that writes design tokens to `:root` (or any scope you name), call it once, and let every rule read the tokens with fallbacks. One call publishes the whole system; overrides happen by re-assigning a variable, never by editing rules.

## The Pattern

```fscss
@define siri-tokens(root: root) {
  `
  @use(root) {
    --siri-wave-container-bg: #000;
    --siri-wave-container-radius: 28px;
    --siri-wave-line-bg: rgba(255,255,255,0.18);
  }
  `
}

@siri-tokens()
```

```css
:root {
  --siri-wave-container-bg: #000;
  --siri-wave-container-radius: 28px;
  --siri-wave-line-bg: rgba(255,255,255,0.18);
}
```

The parameter is a *scope* — default `root`, but it can be any selector. Call `@siri-tokens(.card)` and the same tokens land inside `.card` instead. One define, many scopes.

## Why Scope as a Parameter

Naming the scope as a parameter makes the define reusable:

```fscss
@define theme-tokens(root: :root) {
  `
  @use(root) {
    --bg: #f8fafc;
    --text: #0f172a;
    --accent: #2563eb;
  }
  `
}

@theme-tokens()                 /* writes to :root        */
@theme-tokens(.dark-card)       /* scoped copy for one     */
@theme-tokens([data-theme="x"]) /* scoped by attribute     */
```

Same tokens, scoped by call site. The name `*-root` just says "this is my tokens-to-a-root define".

## Reading with Fallbacks

Rules should read tokens with a default baked in:

```fscss
.card {
  background: var(--bg, #f8fafc);
  color: var(--text, #0f172a);
}
```

If a token is ever missing in some scope, the fallback wins and the design still holds. This is *defensive theming*: safe even before tokens run.

```fscss
@define wave(st: .siri-wave) {
  `
  @use(st) {
    background: var(--siri-wave-container-bg, #000);
    border-radius: var(--siri-wave-container-radius, 24px);
  }
  `
}
```

## The Override Layer

Call tokens first, then re-assign below — CSS variable inheritance does the rest:

```fscss
@siri-tokens()

:root {
  --siri-wave-container-bg: #0a0a12;     /* page-wide override */
}

.spotlight {
  --siri-wave-container-bg: #1a1a2e;     /* one-element override */
}
```

Nothing in the library changes. The token is the single knob.

## Namespacing

Token roots only stay manageable if names don't collide:

- Library namespace: `--siri-wave-*`, `--blob-*` (everything siri-wave owns is greppable under one prefix).
- Semantic helpers that drop into any rule: `--bg`, `--text`, `--accent`.
- Prefix by feature when two subsystems could share a generic name (`--tabs-*`, `--toast-*`).

## The Full Pipeline

```
@fun (palette)  →  @define *-tokens  →  :root  →  var(--x, fallback)  →  components
```

1. Colors and scales live in `@fun` stores (authoring).
2. A `*-root` define publishes them as `--` tokens (publishing).
3. Components read `var(--x, fallback)` (consumption).
4. Overrides re-assign tokens at any scope (theming).

## Summary

- `@define name(root: :root) { \` @use(root) { --token: value; } \` }` publishes tokens.
- Call with a custom scope to place tokens anywhere.
- Always read with `var(--token, fallback)`.
- Override by re-assigning variables, never by editing rules.
- Namespace tokens so the system stays greppable.

---

**Next: [Theme Switching](./theme-switching.md)**