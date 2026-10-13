# 16 — Badges and Tags

Small, loud, and everywhere. Build a family of status badges whose colors come from paired arrays walked with `count()`, plus a removable tag chip — one template, four variants, no copy-paste.

## The Brief

- Status badges for `info`, `success`, `warning`, and `danger`.
- Tint and ink colors generated from paired arrays, not written four times.
- A `dot` modifier with a leading status indicator.
- Removable tags whose close button tints on hover.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #64748b;
  faint: #f1f5f9;
  line: #e2e8f0;
  white: #ffffff;
}

@define badge-base() {
  display: inline-flex;
  align-items: center;
  gap: 0.35rem;
  padding: num(4)px num(10)px;
  border-radius: 999px;
  font-size: num(12 / 16)rem;
  font-weight: 600;
  line-height: 1.4;
  white-space: nowrap;
}

@arr(names[info, success, warning, danger])
@arr(tints[#e0f2fe, #d1fae5, #fef3c7, #fee2e2])
@arr(inks[#075985, #065f46, #92400e, #991b1b])
@arr(i[count(@arr.names!.length, 1)])

.badge-@arr.names[@arr.i[]] {
  $idx: @arr.i[];
  background: @arr.tints[$idx!];
  color: @arr.inks[$idx!];
}

.badge {
  @badge-base()
}

.badge-default {
  @badge-base()
  background: @fun.color.faint.value;
  color: @fun.color.muted.value;
}

.badge-dot::before {
  content: "";
  size: num(6)px;
  border-radius: 50%;
  background: currentColor;
}
```

Tags reuse the same base idea, but with a border and a dismiss affordance:

```fscss
@define tag(ink: #475569) {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  padding: 0.25rem 0.3rem 0.25rem 0.7rem;
  border: 1px solid @fun.color.line.value;
  border-radius: 8px;
  background: @fun.color.white.value;
  color: @use(ink);
  font-size: 0.8rem;
}

.tag { @tag() }

.tag-close {
  display: inline-grid;
  place-items: center;
  size: num(18)px;
  border: none;
  border-radius: 6px;
  background: transparent;
  color: @fun.color.muted.value;
  cursor: pointer;
  line-height: 1;
}

.tag-close:hover {
  background: #fee2e2;
  color: #b91c1c;
}
```

## The HTML

```html
<span class="badge badge-info">Info</span>
<span class="badge badge-success badge-dot">Deployed</span>
<span class="badge badge-warning">Pending</span>
<span class="badge badge-danger badge-dot">Failed</span>
<span class="badge badge-default">Draft</span>

<span class="tag">
  frontend
  <button class="tag-close" aria-label="Remove frontend">×</button>
</span>
<span class="tag">
  accessibility
  <button class="tag-close" aria-label="Remove accessibility">×</button>
</span>
```

## What Happens

- The walk emits four `.badge-*` rules from one template; `$idx` lines tint and ink up at the same position.
- `.badge` and `.badge-default` share `badge-base`, so padding, radius, and type scale change in one place.
- The dot paints with `currentColor`, so it takes whichever status ink the badge already carries.
- `num(12 / 16)rem` keeps the type scale visible as a ratio instead of a magic `0.75rem`.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add an `outline` badge that keeps the ink color but drops the tint background.
2. Add a `+3` overflow badge for when a card holds more tags than fit.
3. Generate the status pill used in the data table from this same array walk.

## Summary

Four statuses, one template. The arrays carry the palette and `count()` decides how many times to stamp it. Change a color once and the whole badge family follows.

---

**Next: [17 — Data Table](../17-data-table.md)**
