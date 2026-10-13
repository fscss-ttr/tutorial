# Event Operators

Matching strings is one thing. Matching *ranges* is where events become genuinely useful — scoring, gauges, thresholds. Numeric parameters support `==`, `>`, `<`, `>=`, and `<=`.

## The Operators

| Operator | Meaning |
|----------|---------|
| `==` | Equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |

They work on numeric parameters passed to the event.

## The Rating Example

```fscss
@event rating(score) {
  if score >= 90 {
    return: #10b981;
  }
  el-if score >= 70 {
    return: #f59e0b;
  }
  el {
    return: #ef4444;
  }
}

.user-score-95 {
  color: @event.rating(95);
}
```

```css
.user-score-95 {
  color: #10b981;
}
```

At 95 → green. At 80 → amber. At 40 → red. No JS, no build script — the stylesheet decides.

## A Progress Bar from a Number

Give the bar a width and a color from the same value:

```fscss
$done: 68;

.progress {
  width: @event.pct($done!)%;
}
```

Wait — you can return the number directly:

```fscss
@event pct(value) {
  if value >= 100 { return: 100; }
  el { return: @use(value); }
}
```

The cleaner pattern combines logic with arithmetic elsewhere. For a bar that caps at 100:

```fscss
@event cap(value) {
  if value > 100 { return: 100; }
  el { return: @use(value); }
}

.bar {
  width: @event.cap(140)%;
}
```

```css
.bar {
  width: 100%;
}
```

## A Density Meter

```fscss
@event density(n) {
  if n < 4    { return: 0.25rem; }   /* sparse  */
  el-if n < 9 { return: 0.75rem; }   /* medium  */
  el { return: 1.25rem; }            /* packed  */
}

.catalog {
  gap: @event.density(@arr.tiles!.length);
}
```

The gap reacts to how many tiles exist, computed at compile time.

## Combining Operators with `num()`

Ranges can be derived, not just written:

```fscss
$breakpoint: 768;
@event layout(width) {
  el-if @use(width) >= num($breakpoint! * 1.5) {
    return: 4;
  }
  el-if @use(width) >= $breakpoint! {
    return: 3;
  }
  el {
    return: 2;
  }
}

.sheet {
  grid-template-columns: repeat(@event.layout(1440), 1fr);
}
```

## Traps

```fscss
/* String vs number: "90" and 90 are different comparisons */
@event a(v) { if v == "90" { return: 1; } el { return: 0; } }

/* Forgetting the cap fallback leaves widths invalid */
@event w(v) {
  if v > 100 { return: 100; }
  el { return: 0; }   /* don't return nothing */
}
```

When a number can be a string or vice versa, be consistent at the call site.

## Summary

- Numeric events support `==`, `>`, `<`, `>=`, `<=`.
- The first true branch wins — order thresholds high to low (or low to high) deliberately.
- Use events for ranges: scores, gauges, caps, densities.
- Always give `el` a sensible fallback.

---

**Next: [Event Themes](./event-themes.md)**