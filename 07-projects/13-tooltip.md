# 13 — Tooltip

A tooltip is a small bubble that appears on hover. Build one bubble define, place it with four modifier classes, and stagger a whole row of tooltips from an array.

## The Brief

- A `.tip` element whose label comes from `data-tip`.
- Four placement modifiers: `--top`, `--right`, `--bottom`, `--left`.
- Reveal on hover and on keyboard focus.
- A row of tooltips with a staggered reveal delay from an array.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  white: #ffffff;
  accent: #2563eb;
}

$gap: 8px;

@define tip-bubble() {
  position: absolute;
  z-index: 20;
  background: @fun.color.ink.value;
  color: @fun.color.white.value;
  font-size: 0.78rem;
  line-height: 1.25;
  padding: 0.4rem 0.65rem;
  border-radius: 6px;
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.15s ease, transform 0.15s ease;
}

@event tip-lift(place) {
  if place: top { return: translate(-50%, -100%); }
  el-if place: bottom { return: translate(-50%, 0); }
  el-if place: left { return: translate(-100%, -50%); }
  el { return: translate(0, -50%); }
}

.tip {
  position: relative;
  display: inline-block;
  border-bottom: 1px dashed @fun.color.accent.value;
  cursor: help;
}

.tip::after {
  @tip-bubble()
  content: attr(data-tip);
}

.tip--top::after {
  top: calc(0px - $gap!);
  left: 50%;
  transform: @event.tip-lift(top);
}

.tip--right::after {
  top: 50%;
  left: calc(100% + $gap!);
  transform: @event.tip-lift(right);
}

.tip--bottom::after {
  top: calc(100% + $gap!);
  left: 50%;
  transform: @event.tip-lift(bottom);
}

.tip--left::after {
  top: 50%;
  right: calc(100% + $gap!);
  transform: @event.tip-lift(left);
}

.tip:hover::after,
.tip:focus-within::after {
  opacity: 1;
}
```

The delay is per-tooltip, so the array walks a set of names and `num()` turns each index into milliseconds:

```fscss
@arr(tips[build, cache, deploy, rollback])
@arr(ti[count(@arr.tips!.length, 1)])

.tip-@arr.tips[@arr.ti[]]:hover::after,
.tip-@arr.tips[@arr.ti[]]:focus-within::after {
  $d: @arr.ti[];
  transition-delay: num($d! * 60)ms;
}
```

## The HTML

```html
<p>
  Hover the terms:
  <span class="tip tip--top tip-build" data-tip="Runs the bundler" tabindex="0">build</span>,
  <span class="tip tip--right tip-cache" data-tip="Reuses parsed modules" tabindex="0">cache</span>,
  <span class="tip tip--bottom tip-deploy" data-tip="Pushes to production" tabindex="0">deploy</span>,
  and <span class="tip tip--left tip-rollback" data-tip="Reverts the last release" tabindex="0">rollback</span>.
</p>
```

## What Happens

- The bubble is one define applied to `.tip::after`; the label is pulled in with `attr(data-tip)`.
- `tip-lift` maps a placement word to a transform, so each modifier rule stays three lines and no geometry is repeated.
- `$gap` plus a single `calc()` keeps the gap identical for all four placements.
- The array and `count()` give each tooltip a 60ms stagger through `num()`.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a rotated `::before` square for an arrow and reuse the same placement classes.
2. Zero the transition inside `@media (prefers-reduced-motion: reduce)`.
3. Infer placement from viewport edges with a few lines of JS that swap the modifier class.

## Summary

One define draws the bubble, an event maps placement to a transform, and the array staggers the row. Placement is a class, not a rewrite.

---

**Next: [14 — Breadcrumb](../14-breadcrumb.md)**
