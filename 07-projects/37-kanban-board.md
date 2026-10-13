# 37 — Kanban Board

A board is columns of cards, and columns are data. Drive the lane count and each lane's accent from arrays, then let a single event decide how loud each column's count badge is.

## The Brief

- A grid of status columns that scrolls horizontally.
- Lane names, lane colours, and the column count from arrays.
- Cards with a coloured priority stripe and a small meta row.
- A per-column count badge that tints by load.

## The FSCSS

```fscss
@fun(color){
  bg: #f1f5f9;
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  line: #e2e8f0;
}

/* token root */
@define kb-root(root: :root) {
  `
  @use(root) {
    --kb-gap: 1rem;
    --kb-radius: 12px;
    --kb-pad: 0.85rem;
  }
  `
}

@kb-root()

@define kb-board() {
  `
  .kboard {
    display: grid;
    gap: var(--kb-gap, 1rem);
    background: @fun.color.bg.value;
    padding: 1.5rem;
    font-family: system-ui, sans-serif;
    align-items: start;
    overflow-x: auto;
  }

  .kb-col {
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.line.value;
    border-radius: var(--kb-radius, 12px);
    padding: 0.75rem;
    display: flex;
    flex-direction: column;
    gap: 0.6rem;
  }

  .kb-col-head {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 0.78rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.4px;
    color: @fun.color.muted.value;
  }

  .kb-card {
    background: #ffffff;
    border: 1px solid @fun.color.line.value;
    border-left: 3px solid @fun.color.muted.value;
    border-radius: 8px;
    padding: var(--kb-pad, 0.85rem);
    color: @fun.color.ink.value;
    font-size: 0.85rem;
    line-height: 1.4;
  }

  .kb-meta {
    margin-top: 0.5rem;
    font-size: 0.72rem;
    color: @fun.color.muted.value;
    display: flex;
    justify-content: space-between;
  }
  `
}

@kb-board()
```

## The Data

The lane array sets the column count and, in a second walk, the colour of each heading. Adding a lane is one array entry:

```fscss
@arr(lanes[Backlog, Doing, Review, Done])
@arr(lane-ink[#94a3b8, #2563eb, #d97706, #10b981])
@arr(li[count(@arr.lanes!.length, 1)])

.kboard {
  grid-template-columns: repeat(@arr.lanes!.length, minmax(220px, 1fr));
}

.kb-col--@arr.lanes[@arr.li[]] .kb-col-head {
  $idx: @arr.li[];
  color: @arr.lane-ink[$idx!];
}
```

Priorities are a second parallel array. Only the stripe changes per card:

```fscss
@arr(pri[low, medium, high])
@arr(pri-ink[#64748b, #d97706, #dc2626])
@arr(pi[count(@arr.pri!.length, 1)])

.kb-card--@arr.pri[@arr.pi[]] {
  $idx: @arr.pi[];
  border-left-color: @arr.pri-ink[$idx!];
}
```

## The Badges

The count badge colour is a threshold, so it belongs in an event rather than another class family:

```fscss
@event load(n) {
  if n: busy { return: #dc2626; }
  el-if n: warm { return: #d97706; }
  el { return: #10b981; }
}

.kb-count { color: @fun.color.muted.value; }
.kb-count--busy { color: @event.load(busy); }
.kb-count--warm { color: @event.load(warm); }
.kb-count--calm { color: @event.load(calm); }
```

## The HTML

```html
<div class="kboard">
  <div class="kb-col kb-col--Backlog">
    <div class="kb-col-head"><span>Backlog</span><span class="kb-count kb-count--busy">7</span></div>
    <div class="kb-card kb-card--low">Rewrite onboarding copy<div class="kb-meta"><span>#214</span><span>2d</span></div></div>
    <div class="kb-card kb-card--medium">Audit empty states<div class="kb-meta"><span>#221</span><span>5d</span></div></div>
  </div>

  <div class="kb-col kb-col--Doing">
    <div class="kb-col-head"><span>Doing</span><span class="kb-count kb-count--warm">3</span></div>
    <div class="kb-card kb-card--high">Fix checkout race<div class="kb-meta"><span>#230</span><span>today</span></div></div>
    <div class="kb-card kb-card--medium">Drop unused tokens<div class="kb-meta"><span>#233</span><span>1d</span></div></div>
  </div>

  <div class="kb-col kb-col--Review">
    <div class="kb-col-head"><span>Review</span><span class="kb-count kb-count--calm">2</span></div>
    <div class="kb-card kb-card--low">Calendar widget<div class="kb-meta"><span>#228</span><span>2d</span></div></div>
  </div>

  <div class="kb-col kb-col--Done">
    <div class="kb-col-head"><span>Done</span><span class="kb-count kb-count--calm">1</span></div>
    <div class="kb-card kb-card--low">Ship dark mode<div class="kb-meta"><span>#201</span><span>—</span></div></div>
  </div>
</div>
```

## What Happens

- `kb-board` sets the grid; the column count comes straight from `@arr.lanes!.length`, so a new lane reflows the board.
- Lane heading colours are one walk over `lane-ink`; the colours travel with the names by index.
- Priority stripes are a second array walk, and only `border-left-color` changes per card.
- `@event load` turns a named badge state into a colour, keeping thresholds out of the class soup.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a drag handle built with `%2(width, height[: 18px;])` and a `cursor: grab`.
2. Generate a `.kb-card--status-*` set from the lane array to tint done cards.
3. Add a collapsed lane variant via a `@define kb-collapsed` block.

## Summary

Lanes are an array, cards are a stripe change, and the one number that carries meaning — load — is an event. The board grows a column without a single new selector.

---

**Next: [38 — Stepper Wizard](../38-stepper-wizard.md)**
