# Advanced Arrays

Most `@arr` usage is read-only: declare, index, check length. Advanced work is *mutation and transformation* — building a list up, pruning it, reordering it, then feeding the result into a loop or a value.

## Append (`!+`)

```fscss
@arr(plan[a, b])

@arr.plan!+[c]       /* a, b, c */
@arr.plan!+[d, e]    /* a, b, c, d, e */
```

Appending is how you compose the item list from multiple sources:

```fscss
@arr icons[home, user]
@arr.icons!+[settings, bell, profile]
```

## Remove (`!-`)

Index-based removal, 1-based:

```fscss
@arr names[a, b, c, d]

@arr.names!-[2]   /* remove second item → a, c, d */
@arr.names!-[1]   /* remove first → c, d */
```

## Sort and Unique on Real Data

```fscss
@arr tags[beta, alpha, beta, gamma, alpha]

.sorted { content: "@arr.tags!.sort"; }   /* alpha, alpha, beta, beta, gamma */
.unique { content: "@arr.tags!.unique"; } /* alpha, beta, alpha, gamma */
```

Sort then unique beats unique then sort for stable lists.

## Shuffle and Reverse

```fscss
@arr deck[ace, king, queen, jack]

.one { content: "@arr.deck!.shuffle"; }  /* random order per compile */
.two { content: "@arr.deck!.reverse"; }  /* jack, queen, king, ace */
```

`.shuffle` is chaos sauce for chips, clouds, and CSS-art — deterministic per compile with the CLI, fresh per render in runtime mode.

## Segment and Surround, Revisited

These shine for building selector-ish and bracket-wrapped output:

```fscss
@arr(attrs[data-a, data-b, data-c])

.selector {
  &:@arr.attrs!.segment { color: #0f172a; }
}
```

`.segment` wraps each in brackets → `[data-a], [data-b], [data-c]`. `.surround(b, a)` wraps with your own delimiters:

```fscss
@arr(keys[key1, key2])

.note { content: "@arr.keys!.surround({{ , }})"; }
```

## Prefix and Unit in Bulk

Generate class or value lists in one step:

```fscss
@arr(breakpoints[sm, md, lg])

.all { content: "@arr.breakpoints!.prefix(min-)"; }   /* min-sm, min-md, min-lg */
.app { content: "@arr.breakpoints!.unit(px)"; }       /* smpx, mdpx, lgpx — beware! */
```

`.unit()` *appends* the unit to each item — the item itself must be a number for `px` to make sense.

## Numeric Summaries

```fscss
@arr sizes[8, 16, 24, 32]

.metrics {
  count: @arr.sizes!.length;   /* 4 */
  total: @arr.sizes!.sum;      /* 80 */
  min:   @arr.sizes!.min;      /* 8 */
  max:   @arr.sizes!.max;      /* 32 */
}
```

Feed `.sum` or `.length` back into `count()` to size a loop by data:

```fscss
@arr(items[a, b, c, d, e])
@arr(indexes[count(@arr.items!.length, 1)])
```

## The Rebuild Rule

Because methods never chain, rebuild through a fresh array:

```fscss
@arr(raw[c, a, b])

@arr.clean[raw]
@arr.clean!+[d]
@arr.clean!-[2]
```

`clean` is now `c, b, d` — an independent list you own.

## Practical: Category Cloud

```fscss
@arr(tags[react, css, fscss, react, dev, css])

@arr tags-final[@arr.tags!.list]
@arr.tags-final!.unique
@arr.tags-final!.sort

.cloud {
  content: "Tags: " @arr.tags-final!.list;
  width: num(@arr.tags-final!.length * 40)px;
}
```

Two lines of mutation produced a clean, sorted, de-duplicated tag list with a data-driven width.

## Summary

- Mutate with `!+[...]` (append) and `!-[n]` (remove by 1-based index).
- `sort`, `unique`, `shuffle`, `reverse` reorganize; `segment`, `surround` reshape.
- `sum`, `min`, `max`, `length` summarize; feed them into `count()` and `num()`.
- Methods don't chain — rebuild through named arrays.

---

**Next: [Parallel Indexes](./parallel-indexes.md)**
