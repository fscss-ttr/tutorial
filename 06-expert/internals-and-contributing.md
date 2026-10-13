# Internals & Contributing

FSCSS is an open compiler heard louder than it is seen. Understanding how it works — and where to pitch in — is the real expert cred. No source-diving required; the architecture is small enough to hold in one head.

## The Pipeline

FSCSS compiles in three passes, roughly:

1. **Parse** — read the `.fscss` source, recognise declares (`$`, `@define`, `@fun`, `@arr`, `@event`, `str()`) and rules.
2. **Resolve** — expand calls in order: imports first (merge), then variables (`$name!`), then defines/`@fun`/arrays, then functions (`num()`, `count()`, `rpt()`…).
3. **Emit** — write standard CSS. Variables inlined, vendor prefixes expanded, keyframes placed, `*` and `@event` resolved.

Order of resolution matters: `@import` runs before anything downstream sees the definitions, so a file must import what a later rule uses.

## The Runtime and CLI Share the Core

`runtime.js`, `esm.js`, and the CLI are all fronts for the same compiler:

| Front | Runs it | When |
|-------|---------|------|
| `runtime.js` | Auto, on page load | Browser, prototyping |
| `esm.js` | On `xfscss.reboot()` | Browser, manual/dynamic |
| `index.js` (npm entry) | On demand | Node, CLI tooling |
| CLI | On command | Builds |

If you fix a parsing bug, every front gets it — one compiler, many drivers.

## Why Output Stays Deterministic

Outside `@random`/`.shuffle`, compilation is a pure function of source: same in, same out. That determinism is why compiled-CSS snapshot tests are trustworthy, and why registry modules don't surprise you at build time. Randomness is explicitly confined to the values you marked.

## Observing Internals Without Reading Them

You already have the introspection tools:

```fscss
exec(_log, 'tokens done')               /* build tracing  */
exec(_warn, 'legacy token in use')       /* deprecations   */
```

Plus the lessons you've built: compiled-CSS diffing, `exec()` breadcrumbs, and golden files are how experts verify what the compiler did.

## Contributing

- **Docs and tutorials** — published openly at [dev.to](https://dev.to/fscss-ttr). Tutorials, guides, and deep-dives are always welcome.
- **Bugs and features** — GitHub issues and discussions under the [FSCSS repo](https://github.com/fscss-ttr/FSCSS).
- **Contributing guide** — the project's CONTRIBUTING file lays out how PRs are structured.
- **Node/packages** — `fscss` on npm is the package; the entry points and versioning live there.

A first contribution can be as small as a docs page or a reproducible bug report.

## The Ecosystem View

FSCSS on GitHub is "Figured Shorthand CSS", conceived 2022, published to npm in 2025, v1.1.x adding arrays/`@event`/`pattern()`, and v1.2.x consolidating the browser entry points and adding property shorthands. The project evolves in public — watching the changelog is the fastest way to stay current.

## Checklist for "I'm Expert Now"

- [ ] I can explain what `@use(param)` does and why bare refs fail.
- [ ] I can hand-trace an array loop output.
- [ ] I know when the runtime belongs and when it doesn't.
- [ ] I can snapshot and diff compiled output.
- [ ] I've shipped (or can shape) a module others import.

## Summary

- FSCSS = parse → resolve → emit; imports resolve first.
- The CLI and both browser entry points share one compiler core.
- Deterministic output (minus explicit `@random`) is why testing works.
- Contribute via GitHub issues/PRs and dev.to docs.
- The fastest upgrade path is the changelog; the deepest is the source.

---

**Next: [Projects](../07-projects/README.md)**