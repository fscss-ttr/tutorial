# 24 — Timeline

A timeline is a vertical line with events hanging off it, alternating left and right. The line is easy; the work is spacing the items, offsetting them to their side, and keeping the markers aligned — all of which FSCSS can generate.

## The Brief

- A vertical spine with a marker per entry.
- Entries alternate left and right on wide screens, stack left on narrow.
- Colors per phase from a parallel array.
- The connector line and spacing derived, not hardcoded per item.

## The FSCSS

```fscss
@fun(color){
  line: #cbd5e1;
  ink: #0f172a;
  muted: #64748b;
  surface: #ffffff;
}

@fun(space){
  gap: 2rem;
  dot: 14;
}

@define entry() {
  position: relative;
  background: @fun.color.surface.value;
  border: 1px solid rgba(15, 23, 42, 0.08);
  border-radius: 12px;
  padding: 1rem 1.25rem;
  box-shadow: 0 2px 8px rgba(15, 23, 42, 0.06);
}

.entry-date {
  font-size: 0.8rem;
  font-weight: 600;
  color: @fun.color.muted.value;
  text-transform: uppercase;
  letter-spacing: 0.04em;
}

.entry-body {
  color: @fun.color.ink.value;
  line-height: 1.55;
}
```

The spine and the alternating offsets are generated from an index array:

```fscss
@arr phases[design, build, ship, learn, scale]
@arr colors[#2563eb, #7c3aed, #0ea5e9, #10b981, #f59e0b]
@arr i[count(@arr.phases!.length, 1)]

@define spine() {
  `
  .timeline {
    position: relative;
    max-width: 720px;
    margin: 0 auto;
    padding: 2rem 1rem;
  }

  .timeline::before {
    content: "";
    position: absolute;
    left: num(@fun.space.dot.value / 2)px;
    top: 0;
    bottom: 0;
    width: 2px;
    background: @fun.color.line.value;
  }

  .timeline-item {
    @entry();
    margin-bottom: @fun.space.gap.value;
    margin-left: num(@fun.space.dot.value * 2)px;
  }

  @media (min-width: 768px) {
    .timeline::before { left: 50%; }
    .timeline-item { width: 44%; }
    .timeline-item:nth-child(odd) { margin-left: 0; margin-right: 56%; }
    .timeline-item:nth-child(even) { margin-left: 56%; }
  }
  `
}

@spine()
```

Each phase gets its marker color, keyed to the same arrays:

```fscss
.timeline-item.phase-@arr.phases[@arr.i[]] {
  $idx: @arr.i[];
  border-left: 4px solid @arr.colors[$idx!];
}

.timeline-item.phase-@arr.phases[@arr.i[]]::after {
  content: "";
  position: absolute;
  top: 1rem;
  margin-left: -1.75rem;
  width: num(@fun.space.dot.value)px;
  height: num(@fun.space.dot.value)px;
  border-radius: 50%;
  background: #fff;
  border: 3px solid @arr.colors[@arr.i[]];
}
```

## The HTML

```html
<ol class="timeline">
  <li class="timeline-item phase-design">
    <div class="entry-date">Jan 2026</div>
    <div class="entry-body">Wireframes and the first token pass.</div>
  </li>
  <li class="timeline-item phase-build">
    <div class="entry-date">Mar 2026</div>
    <div class="entry-body">Component library and dark mode.</div>
  </li>
  <li class="timeline-item phase-ship">
    <div class="entry-date">Jun 2026</div>
    <div class="entry-body">Public beta.</div>
  </li>
</ol>
```

## What Happens

- `@entry()` is the card shape; the timeline positioning stays in the block define so the two concerns don't tangle.
- `.timeline::before` is drawn once and stretched `top: 0; bottom: 0`, so the spine never needs a height.
- `nth-child(odd/even)` alternates the offsets, which is cheaper than a modifier class per entry.
- `phase-*` and the marker colors come from parallel arrays — the same index drives the border and the dot, so a new phase is one item in each array.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Animate the spine in with a `scaleY` transition on scroll.
2. Add a compact single-column variant under 600px with a smaller dot.
3. Drive the marker color from an `@event` keyed on status instead of an array.

## Summary

One block define draws the spine and alternation; parallel arrays color every entry. The timeline is geometry plus aligned data.

---

**Next: [25 — Notification Toast](../25-notification-toast.md)**
