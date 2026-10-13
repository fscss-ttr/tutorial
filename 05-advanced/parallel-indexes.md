# Parallel Indexes

The single most powerful pattern in FSCSS is *two arrays aligned by position* — a list of names and a list of values that match up index-for-index. Together they behave like the rows of a table, and a `count()` array walks them.

## The Pattern

```fscss
@arr colors[magenta, cyan, green]
@arr values[#ff2ecb, #22d3ee, #4ade80]
@arr indexes[count(3, 1)]
```

`colors[1]` pairs with `values[1]`, and so on. Now one loop touches both:

```fscss
.swatch.@arr.colors[@arr.indexes[]] {
  background: @arr.values[@arr.indexes[]];
}
```

```css
.swatch.magenta { background: #ff2ecb; }
.swatch.cyan    { background: #22d3ee; }
.swatch.green   { background: #4ade80; }
```

Three rules, zero copy-paste, one source of truth for the pairing.

## A Real Example from siri-wave

The wave library generates six blob variants exactly this way — a names array and a counter:

```fscss
@define blob-variants(st: .blob) {
  @arr siri-colors[magenta, cyan, green, purple, orange, blue]
  @arr siri-colors-i[count(6, 1)]

  @siri-blob-variant(
    @use(st).@arr.siri-colors[@arr.siri-colors-i[]],
    @arr.siri-colors[@arr.siri-colors-i[]]
  )

  @use(st){
    /* a block is required so the walk fires */
    --siri-loop-index: @arr.siri-colors-i[];
  }
}
```

Read `@arr.siri-colors[@arr.siri-colors-i[]]` out loud: *the siri-colors item at the index siri-colors-i is currently pointing at*. As `siri-colors-i` walks 1, 2, 3…, the lookup walks magenta, cyan, green…

## More Than Two Arrays

Nothing stops at two. A data table with any number of columns:

```fscss
@arr slugs[primary, danger, success]
@arr bg[#2563eb, #dc2626, #10b981]
@arr fg[#ffffff, #ffffff, #0f172a]
@arr gap[4, 6, 3]
@arr i[count(3, 1)]

.pill.@arr.slugs[@arr.i[]] {
  background: @arr.bg[@arr.i[]];
  color: @arr.fg[@arr.i[]];
  gap: num(@arr.gap[@arr.i[]] * 4)px;
}
```

Five arrays, one index, one loop. The "columns" are kept in lockstep because you authored them in the same order.

## Alignment Is a Contract

The whole trick depends on alignment:

- Add an item to `colors` and **must** add a matching item to every parallel array.
- Keep them in the same order, always.
- Reordering one array without the others breaks every mapping.

Generate parallel arrays from `count()` when the values are numeric and predictable, and add a comment naming the "columns" so future you keeps them aligned:

```fscss
/* slots:  name  |  size  |  speed */
@arr slots[hero, tile, chip]
@arr sizes[240, 120, 64]
@arr speeds[1.4, 1.0, 0.6]
```

## Failing Loudly

A mismatch doesn't crash — it just produces a rule with the wrong value. That's the quiet failure mode. Guard it in your head:

- `num(@arr.sizes[@arr.i[]] * 2)` with a non-numeric cell in `sizes` will print the cell verbatim.
- Out-of-range index → empty output, not an error.

## Summary

- Align a names array with value arrays, position for position.
- Drive both with one `count()` index array.
- Use a block (a `@use(st){ }` or selector-bearing rule) to make the walk fire.
- Alignment is a contract — comment the columns, keep order stable.

---

**Next: [Generated Utilities](./generated-utilities.md)**