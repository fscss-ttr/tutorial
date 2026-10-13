# Module Design

A module is a facet of FSCSS exported for others. The expert bar isn't "it works on my page" — it's "someone can `@import((*) from my-module)` with no instructions and no surprises." That takes discipline: namespaces, explicit exports, zero side effects, and honest documentation.

## The Contract

A clean module promises four things:

1. **It imports without side effects.** Nothing leaks, no output appears just by being named.
2. **Every export is namespaced.** No generic `button`, no bare `card`.
3. **It runs on command.** Defines emit when called, never on import.
4. **It has one entry point.** `_index.fscss` is the face; consumers never reach into internals.

## The Layout

```
my-module/
├── _tokens.fscss      # @fun stores + *-root define
├── _buttons.fscss     # btn-* defines
├── _panels.fscss      # panel-* defines
├── _index.fscss       # the public entry point
└── README.md          # usage + version
```

Everything under `_` is internal by convention; `_index.fscss` is what `from` points at.

## Namespacing Everything

```fscss
@define btn-base()       { /* my-module */ }
@define btn-primary()    { /* my-module */ }
@define panel-shell()    { /* my-module */ }

@fun(mod-color) { ... }
@fun(mod-space) { ... }
```

CSS tokens too:

```fscss
:root {
  --mod-color-primary: #2563eb;
  --mod-space-md: 1rem;
}
```

`mod-` (or your library name) is the namespace. A consumer greps `--mod-` and sees every knob.

## Side-Effect-Free Entry

`_index.fscss` exports, it does not execute:

```fscss
/* _index.fscss */
@import((exec(_tokens.fscss)))
@import((exec(_buttons.fscss)))
@import((exec(_panels.fscss)))
```

Imports are bundled behind the `exec()` calls so selective consumers can still import names individually without pulling everything.

## No Leaks, Then Make It Useful

A module that only *exists* is safe but useless. The useful pattern is one-call activation:

```fscss
@define mod-bundle() {
  `
  :root {
    --mod-color-primary: @fun.mod-color.primary.value;
  }
  @keyframes mod-rise { ... }
  `
}
```

Now importing the module does nothing, and `@mod-bundle()` does everything — publish tokens, drop keyframes, wire defaults. Consumers opt in, and nothing surprises them.

## Version and Advertise

```fscss
@define mod-info() {
  exec(_log, "my-module v1.0.0")
}
```

Point the README at the real lifecycle:

```markdown
# my-module

- Import: `@import((*) from my-module)`
- Activate: `@mod-bundle()`
- Version: 1.0.0 — additive changes only within 1.x
- Deprecations: `exec(_warn, ...)` one minor before removal
```

## Test It as a Consumer Would

Create a scratch page that imports nothing but your module:

```fscss
@import((*) from my-module)

.spot {
  @btn-primary()
  @panel-shell()
  @mod-bundle()
}
```

If this compiles clean and the output matches your README, the module passes its own smoke test.

## Checklist

1. Import does nothing; calls do things.
2. Exports and CSS tokens are namespaced.
3. One public entry point; internals hidden behind `_`.
4. Versioned, documented, with `exec(_warn)` deprecation path.
5. Consumer-style smoke test passes before publish.

## Summary

- A clean module imports without side effects and runs on command.
- Namespace every export and every `--token`.
- `_index.fscss` is the public face; `exec()` keeps imports honest.
- Activate via one bundle define; advertise via README and version logs.
- Smoke-test as a stranger would before publishing.

---

**Next: [Registry Imports](./registry-imports.md)**