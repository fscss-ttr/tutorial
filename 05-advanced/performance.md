# Performance

"FSCSS is lightweight" only stays true if you keep it that way. Performance is mostly *what you don't generate*: lean imports, purposeful loops, and a compiled artifact that ships without a runtime.

## Compile, Don't Ship the Runtime

The single biggest performance decision: for production, compile ahead of time.

```bash
fscss style.fscss style.css
```

```html
<!-- Production -->
<link rel="stylesheet" href="style.css">
```

No `runtime.js`, no browser compile cost, no FSCSS source exposing itself to users. Browser compilation is for prototyping and learning only.

## Named Imports Over Wildcards

`@import((*) from any-lib)` is the "include it all, sort it later" of FSCSS:

```fscss
@import((buttons, forms) from ui-kit)   /* small, intent-clear   */
@import((*) from ui-kit)                 /* everything, all the time */
```

Wildcard is fine for tiny kits; named imports keep production output lean and make dependencies obvious.

## Generate Only What You Ship

Generated utilities are free *to write* but not free *to emit*:

```fscss
/* 48 classes for a design that uses five */
@arr(n[1,2,3,4,5,6,7,8,9,10,11,12])
@arr(i[count(12,1)])
.mt-@arr.n[@arr.i[]] { margin-top: num(@arr.n[@arr.i[]] * 4)px; }
```

Rule of thumb: if your page uses three of a class family, write three rules. Generation shines when *most* of the family ships.

## Watch the Runtime Re-Picks

`@random` and `.shuffle` are free at CLI compile time (one pick, baked in) but **run per render** in the browser runtime. On an animation-heavy page in runtime mode, every rule with `@random` is re-evaluated on every render. Keep random on static decoration, or switch to CLI for anything moving.

## Keep Defines Small (Compile Speed ≠ Output Size)

Small defines compile faster and produce no extra output:

- Deeply nested define calls add compile overhead per resolution.
- Blocks that emit what the page never uses bloat both.

Use the compiler's own tool to see it: `exec(_log, 'build done')`, and inspect the compiled CSS size before you call it done.

## Measure

```bash
# Before and after a change
fscss style.fscss style.css
(Get-Item style.css).Length
ls -la dist
```

Track the number. A rule of thumb: your compiled page CSS should be a few KB of *used* styles, not tens of KB of generated family.

## The Runtime/CLI Split, Restated

| Concern | Browser runtime | CLI |
|---------|-----------------|-----|
| Compile cost | On every page load | Zero (pre-built) |
| `@random` | Re-picked each render | Picked once |
| Output served | FSCSS source + compiler | Plain CSS |
| Best for | Prototyping, learning | Production |

## Checklist

1. Production ships compiled CSS, no runtime.
2. Named imports over `(*)`.
3. Generate families you actually use.
4. Keep random/shuffle on decoration or compile-only.
5. Measure `style.css` after changes.

## Summary

- Ship compiled CSS in production; the runtime is a prototyping tool.
- Import selectively, generate deliberately.
- Random is cheap at compile time, costly at render time.
- Measure the compiled artifact and keep it proportional to what the page uses.

---

**Next: [FSCSS with Frameworks](./frameworks.md)**