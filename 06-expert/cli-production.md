# CLI Production Output

The CLI's job is to turn `.fscss` into deployable CSS — no runtime, no source, just one clean file the browser loads instantly. This lesson is the production checklist around that one command.

## The Command

```bash
fscss src/styles.fscss dist/styles.css
```

Button one. The output is plain CSS: every variable inlined, every define expanded, every import merged. It is what the browser would have gotten from the runtime, frozen at build time.

## Then Serve Plain CSS

```html
<link rel="stylesheet" href="dist/styles.css">
```

No FSCSS script on the page. Benefits:

- Zero compile cost at page load.
- No FSCSS source shipped to users.
- `@random` deterministic (picked once at build).
- Works behind any static host, CDN, or CMS.

## The Production Project Layout

```
project/
├── src/
│   └── styles.fscss          # imports everything
├── dist/
│   └── styles.css            # compiled, deployable
└── package.json
```

```fscss
/* src/styles.fscss — single entry */
@import(exec(_tokens.fscss))
@import(exec(_base.fscss))
@import(exec(_components.fscss))
```

Compile one entry, get one output. Multiple `dist` files only when different pages genuinely need different CSS.

## Wiring Into the Build

```json
{
  "scripts": {
    "build:css": "fscss src/styles.fscss dist/styles.css"
  }
}
```

Chain it before every deployable build:

```json
{
  "scripts": {
    "build": "npm run build:css && npm run build:app"
  }
}
```

In CI, it's just another step:

```yaml
- run: npm run build:css
```

## Verifying the Output

Before you promote a build, look at what you're shipping:

```bash
fscss src/styles.fscss dist/styles.css
(Get-Item dist/styles.css).Length
```

- **No `$` remains** — variables should be gone, einlined as values.
- **No `@arr`/**@`define` remains** — everything expanded.
- **Random values are baked** — `@random` resolved once.
- **Size is proportional** — a floaty component lib shouldn't ship a 200 KB stylesheet.

Spot-check a define call in the compiled output:

```bash
rg "btn-primary" dist/styles.css
```

## Keeping the Runtime Out

Production pitfalls to avoid:

```html
<!-- NEVER ship this in production -->
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js"></script>
```

If you catch the runtime in a production bundle, you've shipped a compiler to your users. The runtime is a dev/learning tool; production is compiled CSS or nothing.

## The Rebuild Discipline

In production you recompile when source changes, not when pages load:

- Rebuild on every deploy.
- Snapshot the compiled CSS in CI (see [Testing FSCSS](../05-advanced/testing-fscss.md)).
- Cache aggressively — the file is static.

## Summary

- `fscss src/styles.fscss dist/styles.css` → deployable plain CSS.
- One entry point per output; imports merge at compile time.
- Chain the compile into every build and CI pipeline.
- Verify: no `$`/`@arr`/`@define` in output, random baked, size sane.
- Runtime stays out of production; production is compiled CSS or nothing.

---

**Next: [Internals & Contributing](./internals-and-contributing.md)**