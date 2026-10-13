# Registry Imports (`fscss:name`)

From v1.2.1+, FSCSS can resolve modules published on the FSCSS registry. Built-in libraries use the `fscss:` prefix; published community libraries (like `siri-wave`) import by name. This is how a design system travels without bundling.

## The Two Kinds of Names

| Name | Resolves to |
|------|-------------|
| `fscss:micros` | A built-in library shipped with FSCSS |
| `siri-wave` | A published community module |
| `./path/_file.fscss` | A local file |
| `"https://…"` | A remote URL |

## Importing a Built-In Lib

```fscss
@import((mirror) from fscss:micros)
```

`fscss:micros` is the registry namespace for FSCSS's own micro-libraries. Import a named export:

```fscss
@import((collect) from fscss:micros)
```

Or everything the lib ships:

```fscss
@import((*) from fscss:micros)
```

Wildcards on built-ins are fine — they're small and curated.

## Importing a Published Module

```fscss
@import((*) from siri-wave)
```

Community modules publish under a name, host the entry point, and consumers pull them like any library. The registry replaces "copy the `.fscss` file into your repo" with a one-liner import.

## Version Pinning (Registry-Style)

For remote or package-hosted modules, pin a version when you need reproducibility:

```fscss
@import((*) from "https://cdn.example.com/packages/siri-wave@1.2.0/index.fscss")
```

For named registry imports, the registry holds the current published version — upgrade explicitly later if you must.

## Selective from a Registry Module

Don't import a whole library to use one component:

```fscss
@import((btn, btn-base) from my-module)
```

If `my-module` exports those names, only they are pulled. This is registry-style selective importing, identical to local modular importing — just resolved by name.

## Aliasing Registry Names

Clashes happen across libraries too:

```fscss
@import((theme as brand-light) from fscss:micros)
@import((theme as brand-dark) from vendor-themes)
```

## Trouble in Registry Land

- **Name not found** — the module isn't published (or the name is misspelled).
- **Export not found** — the module publishes but does not export that define/store; check its README.
- **Behavior drift** — registry modules update silently; pin or snapshot compiled CSS in CI.

Use `exec(_log, ...)` at your entry to confirm which module resolved:

```fscss
exec(_log, 'using siri-wave from registry')
@import((*) from siri-wave)
```

## Summary

- `fscss:name` imports built-in libraries; published modules import by name.
- Selectively and alias exactly like local imports.
- Pin versions for reproducibility where a URL is involved.
- Registry modules update under you — snapshot compiled output to stay safe.

---

**Next: [runtime.js vs esm.js](./runtime-vs-esm-deep.md)**