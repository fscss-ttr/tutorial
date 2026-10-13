# 39 — Rating Stars

Stars that only fill completely are a lie; scores are fractional. Build two identical glyph layers, clip the amber one to a computed width, and drive the score bands from an array.

## The Brief

- A five-star row that fills to any fraction.
- An empty layer and a filled layer, the latter clipped by `--rating`.
- Score bands generated from one array of percentages.
- A verdict label coloured by a score event.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #64748b;
  empty: #cbd5e1;
  star: #f59e0b;
  low: #dc2626;
  mid: #d97706;
  high: #10b981;
}

/* token root */
@define rating-root(root: :root) {
  `
  @use(root) {
    --star-size: 1.4rem;
    --star-gap: 2px;
  }
  `
}

@rating-root()

$score: 4;
$total: 5;

@event verdict(score) {
  if score: low { return: #dc2626; }
  el-if score: mid { return: #d97706; }
  el { return: #10b981; }
}

@define rating() {
  `
  .rating {
    display: inline-flex;
    align-items: center;
    gap: 0.6rem;
    font-family: system-ui, sans-serif;
  }

  .rating-stars {
    position: relative;
    display: inline-block;
    font-size: var(--star-size, 1.4rem);
    line-height: 1;
    letter-spacing: var(--star-gap, 2px);
  }

  .rating-stars::before {
    content: "rpt(5, '\2605')";
    color: @fun.color.empty.value;
  }

  .rating-stars::after {
    content: "rpt(5, '\2605')";
    position: absolute;
    top: 0;
    left: 0;
    overflow: hidden;
    white-space: nowrap;
    width: var(--rating, 0%);
    color: @fun.color.star.value;
  }

  .rating-verdict {
    font-size: 0.85rem;
    font-weight: 600;
    color: @fun.color.muted.value;
  }
  `
}

@rating()
```

## The Data

One class per score band, generated from two parallel arrays. The star count follows the array length:

```fscss
@arr(scores[one, two, three, four, five])
@arr(widths[20, 40, 60, 80, 100])
@arr(ri[count(@arr.scores!.length, 1)])

.rating--@arr.scores[@arr.ri[]] {
  $idx: @arr.ri[];
  --rating: num(@arr.widths[$idx!] * 1)%;
}
```

The verdict colour is a band, so it is an event rather than a sixth class of its own:

```fscss
.rating-verdict--low { color: @event.verdict(low); }
.rating-verdict--mid { color: @event.verdict(mid); }
.rating-verdict--high { color: @event.verdict(high); }
```

## The HTML

```html
<div class="rating rating--four">
  <span class="rating-stars"></span>
  <span class="rating-verdict rating-verdict--high">4.0 · Great</span>
</div>

<div class="rating rating--two">
  <span class="rating-stars"></span>
  <span class="rating-verdict rating-verdict--low">2.0 · Meh</span>
</div>
```

## What Happens

- Empty and filled layers hold the exact same glyphs; only `color` and `width: var(--rating)` differ, so fractions work with no extra markup.
- `rpt(5, ...)` repeats the star glyph in both `content` strings — one source for the shape, two paints.
- `@arr.scores` and `@arr.widths` generate one `--rating` class per band; extending the array adds half-star bands.
- `@event verdict` maps a named band to the label colour, while `--star-size` and `--star-gap` resize the whole row.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a `.rating--interactive` variant where hovering a star sets `--rating` via `:has()`.
2. Swap the glyph for a heart by editing the two `content` strings only.
3. Generate half-star bands by prepending `10, 30, 50, 70, 90` to the `widths` array.

## Summary

The score is a number, so it becomes a width. Two glyph layers, one array of bands, one event for the word underneath — the star count never enters the stylesheet as a hardcoded five.

---

**Next: [40 — File Upload](../40-file-upload.md)**
