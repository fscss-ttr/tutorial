# Generated Utilities

Four lines of FSCSS can emit forty utility classes. That is the whole pitch: write the *rule*, let arrays and `count()` write the *many*.

## A Margin Scale

```fscss
@arr n[1, 2, 3, 4, 5, 6, 7, 8]
@arr i[count(8, 1)]

.mt-@arr.n[@arr.i[]] {
  margin-top: num(@arr.n[@arr.i[]] * 4)px;
}
```

```css
.mt-1 { margin-top: 4px; }
.mt-2 { margin-top: 8px; }
/* … */
.mt-8 { margin-top: 32px; }
```

Add `.mb-`, `.ml-`, `.mr-` by duplicating the block with a different property — or better, generate all four sides:

```fscss
.m@arr.q[@arr.i[]]{ margin: num(@arr.n[@arr.i[]] * 4)px; }
```

Use a `q` array `[t, b, l, r]` plus a property map… that's over-engineering. FSCSS loops write *length*; write four explicit side loops and keep them obvious.

## A Type Scale

Non-linear scales are a `num()` job:

```fscss
@arr step[1, 2, 3, 4, 5, 6]
@arr i[count(6, 1)]

.text-@arr.step[@arr.i[]] {
  font-size: num(<12 + @arr.step[@arr.i[]] * 2>)px;
}
```

```css
.text-1 { font-size: 14px; }
.text-2 { font-size: 16px; }
.text-3 { font-size: 18px; }
.text-4 { font-size: 20px; }
```

Watch the angle brackets — `num()` groups with `< >`, never parentheses.

## A Grid Column System

```fscss
@arr col[1, 2, 3, 4, 6, 12]
@arr i[count(6, 1)]

.col-@arr.col[@arr.i[]] {
  grid-column: span @arr.col[@arr.i[]];
}
```

Note the deliberate list: 1, 2, 3, 4, 6, 12 — a *designed* system, not a boring 1..12.

## Z-Index and Opacity

```fscss
@arr z[1, 2, 3, 4, 5, 6]
@arr zi[count(6, 1)]

.z-@arr.z[@arr.zi[]] {
  z-index: num(@arr.z[@arr.zi[]] * 10);
}

@arr o[1, 2, 3, 4, 5, 6, 7, 8, 9]
@arr oi[count(9, 1)]

.op-@arr.o[@arr.oi[]] {
  opacity: num(@arr.o[@arr.oi[]] * 0.1);
}
```

`.op-1` … `.op-9` → 0.1 … 0.9. Cell values in the array are the inputs; the formula turns them into percentages, decimals, or pixels.

## Themes as Generated Scales

Don't stop at utilities. A whole swatch set:

```fscss
@arr shade[50, 100, 200, 300, 400, 500, 600, 700]
@arr si[count(8, 1)]

.bg-blue-@arr.shade[@arr.si[]] {
  background: hsl(@arr.shade[@arr.si[]] 100%, 50%);
}
```

(That produces wildly loud colors — the *point* is the pattern, not the palette. Use a real palette array in production.)

## When Generation Is Wrong

- **You need every class** — generation saves keystrokes.
- **You'll ship them all** — generated CSS is identical to hand-written; size is on you.
- **You only use three of them** — write three rules. Don't generate for the sake of it.

## Checklist

1. One `n` (names) array, one `i` (`count(...)`) array.
2. Index both with the same `@arr.x[@arr.i[]]`.
3. Wrap the math in `num()` with `< >` grouping and the unit after `)`.
4. Verify the compiled output once — generated scales hide typos well.

## Summary

- Arrays + `count()` generate full utility systems on demand.
- Numeric formulas in `num()` shape the scale.
- Design the item list (don't just count 1..N) for purposeful systems.
- Generate what you'll ship; hand-write what you won't.

---

**Next: [Random in Depth](./random-advanced.md)**