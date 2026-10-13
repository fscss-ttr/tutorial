# Auto-Indexing & Loops

FSCSS has no `for` or `each` loop. Instead you generate repetition with arrays and `count()`: a value array, a counter array, and a block that walks them together. Once it clicks, it is more powerful than a loop — it is a loop that already has its data.

## `count()`

`count(limit)` produces a comma-separated sequence starting at 1. `count(limit, step)` starts at a custom step.

```text
count(5)      → 1, 2, 3, 4, 5
count(10, 2)  → 2, 4, 6, 8, 10
```

Feed it into an array:

```fscss
@arr(num[count(5)])
@arr(even[count(10, 2)])
```

`limit` can also be an array length: `count(@arr.colors!.length, 1)`.

## The Empty Bracket Walk

`@arr.name[]` is the workhorse. Inside a selector it walks every item, emitting one rule per item:

```fscss
@arr indexes[count(3, 1)]

.loading-dot:nth-child(@arr.indexes[]) {
  animation-delay: @arr.indexes[];
}
```

```css
.loading-dot:nth-child(1) { animation-delay: 1; }
.loading-dot:nth-child(2) { animation-delay: 2; }
.loading-dot:nth-child(3) { animation-delay: 3; }
```

## Pairing a Counter with a Value Array

The real trick is *two arrays aligned by position*: a counter tells you which item, a value array tells you what to use there. Capture the current counter in a local `$` variable so it behaves like a proper loop index:

```fscss
@arr delays[0.1s, 0.3s, 0.5s]
@arr colors[#ef4444, #f59e0b, #10b981]
@arr indexes[count(@arr.colors!.length, 1)]

.loading-dot:nth-child(@arr.indexes[]) {
  $index: @arr.indexes[];
  animation-delay: @arr.delays[$index!];
  background: @arr.colors[$index!];
}
```

```css
.loading-dot:nth-child(1) {
  animation-delay: 0.1s;
  background: #ef4444;
}
.loading-dot:nth-child(2) {
  animation-delay: 0.3s;
  background: #f59e0b;
}
.loading-dot:nth-child(3) {
  animation-delay: 0.5s;
  background: #10b981;
}
```

Read it left to right: *column 1 of colors, column 1 of delays*; *column 2 of colors, column 2 of delays*… Because both arrays were authored in the same order, `$index!` lines them up.

## Generating Whole Rules

The same pattern generates class rules instead of `:nth-child` steps:

```fscss
@arr sizes[0\\.25rem, 0\\.5rem, 0\\.75rem, 1rem]
@arr names[1, 2, 3, 4]
@arr indexes[count(4, 1)]

.mt-@arr.names[@arr.indexes[]] {
  margin-top: @arr.sizes[@arr.indexes[]];
}
```

Each iteration concatenates the selector: `.mt-1`, `.mt-2`, … This is exactly how a spacing utility scale is generated — data in, hundreds of classes out.

## The Loop Engine Gotcha

Array expansion happens inside a **block** (a rule that uses the walk). Selector-bearing calls expand per position; a plain declaration without a block has nowhere to iterate. If you have a loop that "isn't running", make sure something inside triggers the expansion:

```fscss
@define blob-colors(st:.swatch) {
  @arr colors[magenta, cyan, green]
  @arr indexes[count(3, 1)]

  @use(st){
    /* A block is required so the array walk fires */
    --swatch-name: @arr.colors[@arr.indexes[]];
  }
}
```

`@use(st){ ... }` gives the array walk a block to iterate over. The property it writes may be meaningless; the *iteration* is the point.

## When to Reach for a Loop

- Spacing and type scales (`.mt-1` … `.mt-12`).
- CSS-art and chip boards (one rule per item).
- Staggered animations (a delay array indexed by position).
- Parallel property sets (color + delay + size, all keyed to the same index).

## Summary

- `count(limit[, step])` builds counter arrays.
- `@arr.name[]` walks every item; a counter array + value array aligned by position is the loop.
- Capture the index in a `$index: @arr.name[];` local, then read `@arr.other[$index!]`.
- Loops fire inside blocks — selector-bearing rules and `@use(st){ }` blocks.

---

**Next: [Imports](./imports.md)**