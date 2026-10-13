# 42 — Stats Cards

A row of metric cards is the dashboard's first impression: label, big number, and a delta that tells you which way things moved. Build the cards from data and let an event decide whether a delta is good news or bad.

## The Brief

- A responsive row of stat cards.
- Label, value, and a signed delta per card.
- Up/neutral/down tones from an event.
- A subtle sparkline strip at the bottom of each card.

## The FSCSS

```fscss
@fun(color){
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  up: #10b981;
  down: #ef4444;
  line: #e2e8f0;
}

@fun(space){
  pad: 1.5rem;
  gap: 1rem;
}

@event delta(direction) {
  if direction: up {
    return: @fun.color.up.value;
  }
  if direction: down {
    return: @fun.color.down.value;
  }
  el {
    return: @fun.color.muted.value;
  }
}

@event marker(direction) {
  if direction: up {
    return: "▲";
  }
  if direction: down {
    return: "▼";
  }
  el {
    return: "•";
  }
}

@define stat(radius: 16px) {
  background: @fun.color.surface.value;
  border: 1px solid @fun.color.line.value;
  border-radius: @use(radius);
  padding: @fun.space.pad.value;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.stat-label {
  font-size: 0.72rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: @fun.color.muted.value;
}

.stat-value {
  font-size: 1.9rem;
  font-weight: 700;
  line-height: 1;
  color: @fun.color.ink.value;
}
```

The cards themselves are parallel arrays — label, value, and direction:

```fscss
/* columns:  label        |  value   |  direction */
@arr labels[Revenue, Active Users, Churn, NPS]
@arr values[18400, 2841, 2.4, 61]
@arr dirs[up, up, down, up]
@arr i[count(@arr.labels!.length, 1)]

.stat-@arr.i[] .stat-delta {
  font-size: 0.8rem;
  font-weight: 600;
  color: @event.delta(@arr.dirs[@arr.i[]]);
}

.stat-@arr.i[] .stat-delta::before {
  content: "@event.marker(@arr.dirs[@arr.i[]]) ";
}

.stat-@arr.i[]::after {
  content: "";
  height: 6px;
  border-radius: 999px;
  background: linear-gradient(
    90deg,
    @fun.color.line.value,
    @event.delta(@arr.dirs[@arr.i[]])
  );
  opacity: 0.5;
}
```

## The Layout

```fscss
@define grid() {
  `
  .stats {
    display: grid;
    grid-template-columns: 1fr;
    gap: @fun.space.gap.value;
  }

  @media (min-width: 640px) {
    .stats { grid-template-columns: repeat(2, 1fr); }
  }

  @media (min-width: 1024px) {
    .stats { grid-template-columns: repeat(4, 1fr); }
  }

  .stats .stat { @stat(); }
  `
}

@grid()
```

## The HTML

```html
<section class="stats">
  <article class="stat stat-1">
    <div class="stat-label">Revenue</div>
    <div class="stat-value">$18,400</div>
    <div class="stat-delta">+12.4%</div>
  </article>
  <article class="stat stat-2">
    <div class="stat-label">Active Users</div>
    <div class="stat-value">2,841</div>
    <div class="stat-delta">+3.1%</div>
  </article>
  <article class="stat stat-3">
    <div class="stat-label">Churn</div>
    <div class="stat-value">2.4%</div>
    <div class="stat-delta">-0.6%</div>
  </article>
  <article class="stat stat-4">
    <div class="stat-label">NPS</div>
    <div class="stat-value">61</div>
    <div class="stat-delta">+4</div>
  </article>
</section>
```

## What Happens

- `@event.delta()` turns a direction name into a color, so `up`, `down`, and anything unknown are handled in one place.
- The `stat-*` rules are generated from `@arr.labels[]`, and each one reads its direction from the aligned `dirs` array — the value and its tone stay paired.
- The sparkline strip is a pseudo-element whose gradient ends on the same delta color, so the tone appears twice from one source.
- The grid is a block define, which is why the two breakpoints sit together instead of scattered.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Replace the free-form values with `num()`-computed dummy data.
2. Add a sparkline from an array of points using a `clip-path` polygon.
3. Add a compact single-line variant with `@event size`.

## Summary

Parallel arrays hold the numbers, one event colors the deltas, and one block define lays out the grid. The dashboard's first row is data all the way down.

---

**Next: [43 — Landing Page](../43-landing-page.md)**
