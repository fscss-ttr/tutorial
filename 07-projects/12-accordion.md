# 12 — Accordion

An accordion is a stack of headers that reveal a panel each. Build the items from an array, keep the open state on one class, and let a block define draw the chevrons.

## The Brief

- A stacked list of items, each a header and a collapsible panel.
- Item and header classes generated from one array.
- A single `is-open` class reveals the panel and flips the chevron.
- Shared panel and chevron rules emitted by a block define.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #64748b;
  line: #e2e8f0;
  surface: #ffffff;
  accent: #2563eb;
}

@arr(sections[shipping, returns, warranty, support])
@arr(si[count(@arr.sections!.length, 1)])

.acc-item-@arr.sections[@arr.si[]] {
  background: @fun.color.surface.value;
  border: 1px solid @fun.color.line.value;
  border-bottom: none;
}

.acc-item-@arr.sections[@arr.si[]]:last-child {
  border-bottom: 1px solid @fun.color.line.value;
}

.acc-head-@arr.sections[@arr.si[]] {
  width: 100%;
  text-align: left;
  padding: 1rem 1.25rem;
  background: transparent;
  border: none;
  color: @fun.color.ink.value;
  font: inherit;
  font-weight: 600;
  cursor: pointer;
}

.acc-head-@arr.sections[@arr.si[]]:hover {
  color: @fun.color.accent.value;
}
```

The panel, the chevron, and the focus ring are identical for every item, so they go in one block define:

```fscss
@define acc-panels() {
  `
  .acc-panel {
    display: none;
    padding: 0 1.25rem 1rem;
    color: @fun.color.muted.value;
    line-height: 1.6;
  }
  .acc-item.is-open .acc-panel { display: block; }
  .acc-head::after {
    content: "+";
    float: right;
    font-weight: 400;
    color: @fun.color.muted.value;
  }
  .acc-item.is-open .acc-head::after {
    content: "–";
    color: @fun.color.accent.value;
  }
  .acc-head:focus-visible {
    outline: 2px solid @fun.color.accent.value;
    outline-offset: -2px;
  }
  `
}

@acc-panels()
```

## The HTML

```html
<div class="acc">
  <div class="acc-item-shipping acc-item is-open">
    <button class="acc-head-shipping acc-head">Shipping</button>
    <div class="acc-panel">Orders ship within two business days.</div>
  </div>
  <div class="acc-item-returns acc-item">
    <button class="acc-head-returns acc-head">Returns</button>
    <div class="acc-panel">Thirty days, no questions.</div>
  </div>
  <div class="acc-item-warranty acc-item">
    <button class="acc-head-warranty acc-head">Warranty</button>
    <div class="acc-panel">Two years against defects.</div>
  </div>
  <div class="acc-item-support acc-item">
    <button class="acc-head-support acc-head">Support</button>
    <div class="acc-panel">Email us and a human answers.</div>
  </div>
</div>
```

## What Happens

- The array stamps the four item and header shells; the panel rules are shared by every item.
- `is-open` is one class toggled in JS — it flips both the panel's display and the chevron glyph.
- The chevron is pure CSS via `::after` inside the block define, so no icon markup is needed.
- The border rhythm uses `border-bottom: none` on each item plus a `:last-child` rule for the closing line.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Animate the panels with a `max-height` transition driven by an `--open-height` custom property.
2. Rebuild it as native `<details>` and `<summary>` to drop the JS class entirely.
3. Feed the same `sections` array into a sitemap so nav and accordion stay in step.

## Summary

Shared panel rules, a single open class, and a block define for the chevrons — the accordion grows one array entry at a time.

---

**Next: [13 — Tooltip](../13-tooltip.md)**
