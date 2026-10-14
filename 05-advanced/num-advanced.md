# Math in Depth

`num()` does compile-time arithmetic. This lesson is about designing real layouts with it — columns, ratios, offsets — and knowing exactly what the compiler will (and won't) compute.

## The Rules, Restated

- Supports `+`, `-`, `*`, `/`.
- Units attach **after** the closing parenthesis.
- Group with `<` and `>` — parentheses are the enemy here.
- Everything inside is evaluated numerically at compile time.

```fscss
.wide {
  width: num(<1200 - 40 * 2>)px;   /* 1120px */
}
```

## A 12-Column Width

```fscss
$cols: 12;
$gap: 24;

.col-4 {
  width: num(< <$cols! - 4> / $cols! * 100>)%;
}
```

Wait — no parentheses. Rewrite with angle brackets, two layers:

```fscss
.col-4 {
  width: num(<100> * <$cols!-4> / $cols!)%;
}
```

`num(<100> * <8> / 12)` → `66.66…` → `66.66…%`. Precedent: `60` in the example.

## Aspect Ratio Without `aspect-ratio`

Sometimes you need the height of a width. Compute it:

```fscss
$w: 320;
.hero {
  height: num($w! * 9 / 16)px;   /* 180px */
}
```

## Offsets From a Known Total

```fscss
.offset {
  left: num(50% - var(--thumb) / 2);
}
```

Mixed units won't resolve — CSS `calc()` is your friend there. For pure numbers, `num()` resolves completely.

## Composing Random + Math

```fscss
textarea {
  max-height: num(@random([40, 10, 5, 0]) + 50);
}
```

Random resolves first, arithmetic follows. This is how you get *randomized but bounded* geometry.

## Loop-Driven Math

Keeping math inside generated utilities:

```fscss
@arr steps[1, 2, 3, 4, 5, 6]
@arr i[count(6, 1)]

@define pad-step(n) {
  `
  .step-@use(n) { padding: num(@use(n) * 4)px; }
  `
}

@pad-step(@arr.steps[@arr.i[]])
```

One define, six calls, a full padding scale.

## What `num()` Won't Do

- **`var()` values** — they're unknown at compile time. Leave them to `calc()`.
- **Em math across unknown base sizes** — sizes are resolved by the browser, not FSCSS.
- **Parentheses** — `< >` only.
- **Strings** — `num("4 * 3")` is not arithmetic.

## Debugging a Wrong Number

```fscss
/* 68% but you expected 70% — check grouping first */
width: num(<100 - 32> * 0.90)%;
```

When output surprises you:

1. Add `exec(_log, "reached .col-4")` to confirm the branch ran.
2. Split the expression into named pieces with `$` variables.
3. Compile to a scratch file and eyeball the resolved numbers.

## Summary

- `num()` resolves `+ - * /` at compile time; `< >` groups, units go after `)`.
- Build column widths, ratios, offsets, and scales from pure numbers.
- Leave `var()` and mixed-unit math to `calc()`.
- Use pieces and `exec()` when the answer comes out wrong.

---

**Next: [Token Roots](./token-roots.md)**
