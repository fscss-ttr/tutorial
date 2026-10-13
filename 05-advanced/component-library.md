# Building Component Libraries

A component library is `@define` + `@fun` + tokens, organized. Each piece stays small, namespaced, and composable; the library promises a clean import and a consistent visual language to every consuming project.

## The Anatomy

```
my-kit/
├── _tokens.fscss     # @fun stores + *-root define
├── _buttons.fscss    # button defines
├── _forms.fscss      # input/checkbox defines
├── _alerts.fscss     # alert/foundation defines
└── _index.fscss      # the public entry point
```

Separate *data* (tokens), *building blocks* (defines), and *entry* (imports).

## Tokens First

```fscss
/* _tokens.fscss */
@fun(color){
  primary: #2563eb;
  primary-strong: #1d4ed8;
  danger: #dc2626;
}

@fun(space){
  sm: 0.5rem;
  md: 1rem;
  lg: 1.5rem;
}

@define kit-tokens(root: :root) {
  `
  @use(root) {
    --kit-color-primary: @fun.color.primary.value;
    --kit-space-md: @fun.space.md.value;
  }
  `
}
```

## Small, Focused Defines

Each define owns one behavior and composes:

```fscss
/* _buttons.fscss */
@define btn-base() {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}

@define btn(variant: primary) {
  @btn-base()
  background: var(--kit-color-@use(variant), #2563eb);
  color: white;
}
```

Consumers call one define per variant, collapse all the base behavior automatically.

## Forms and Alerts

```fscss
@define field() {
  width: 100%;
  padding: var(--kit-space-sm, 0.5rem) var(--kit-space-md, 1rem);
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  font-size: 1rem;
}

@define alert(kind: info) {
  padding: 1rem 1.25rem;
  border-radius: 8px;
  border-left: 4px solid;
}
```

Name the *behavior*, let tokens carry the look.

## The Public Entry

`_index.fscss` is the whole kit's face:

```fscss
@import(exec(_tokens.fscss))
@import(exec(_buttons.fscss))
@import(exec(_forms.fscss))
@import(exec(_alerts.fscss))
```

Consumers import what they need — the whole kit or one piece:

```fscss
@import((*) from my-kit)
/* or selective: */
@import((btn, btn-base) from my-kit)
```

## Namespacing Rules

- Every exported name is prefixed: `btn`, `field`, `alert`.
- Every CSS token is prefixed: `--kit-*`.
- No leaked generic names (`card`, `box`) that collide in the consumer's app.
- One responsibility per define; compose rather than fork.

## Versioning

A library is a contract. Keep it stable:

- Additive changes only within a major version.
- Deprecate with `exec(_warn, …)` before removing.
- Export a `kit-version` define consumers can log:
  ```fscss
  @define kit-version() { exec(_log, "my-kit v1.2.0") }
  @kit-version()
  ```

## The Golden Test

A component library is ready when a consumer writes:

```fscss
@import((*) from my-kit)

.login {
  @btn()
  @field()
  @alert(info)
}
```

…and gets a coherent, themeable component with zero knowledge of the internals.

## Summary

- Organize: tokens → building blocks → entry point.
- Tokens are `@fun` stores published via a `*-root` define as `--kit-*`.
- Defines are small, namespaced, composable behaviors.
- `_index.fscss` is the public API; import selectively or wildcard.
- Version it like the contract it is.

---

**Next: [Grid Layouts](./grid-layouts.md)**