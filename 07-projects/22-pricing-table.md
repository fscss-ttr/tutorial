# 22 — Pricing Table

Three plans, one highlighted, and a list of features that must line up. Build the tiers from parallel arrays so a fourth plan is a matter of adding a column of data, and use an event to decide which tier gets the emphasis.

## The Brief

- Three plan cards side by side, stacking under 800px.
- A "popular" tier that lifts, scales, and gets an accent border.
- A feature list shared by all tiers.
- Prices formatted from a numeric array.

## The FSCSS

```fscss
@fun(color){
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  accent: #2563eb;
  bg: #f8fafc;
}

@fun(space){
  pad: 2rem;
  gap: 1.25rem;
}

@define plan(radius: 16px) {
  background: @fun.color.surface.value;
  border: 1px solid rgba(15, 23, 42, 0.08);
  border-radius: @use(radius);
  padding: @fun.space.pad.value;
  display: flex;
  flex-direction: column;
  gap: @fun.space.gap.value;
}

@event emphasis(plan) {
  if plan: pro {
    return: @fun.color.accent.value;
  }
  el {
    return: transparent;
  }
}
```

The tiers are parallel arrays — names, prices, and which one is featured:

```fscss
@arr plans[starter, pro, studio]
@arr prices[0, 29, 99]
@arr featured[no, yes, no]
@arr i[count(@arr.plans!.length, 1)]

.plan-@arr.plans[@arr.i[]] {
  @plan()
  border-top: 4px solid @event.emphasis(@arr.plans[@arr.i[]]);
}

.plan-@arr.plans[@arr.i[]] .price {
  font-size: 2.25rem;
  font-weight: 700;
  color: @fun.color.ink.value;
}

.plan-@arr.plans[@arr.i[]] .price::before {
  content: "$";
  font-size: 1rem;
  vertical-align: super;
  color: @fun.color.muted.value;
}
```

## The Layout

```fscss
@define tiers() {
  `
  .tiers {
    display: grid;
    grid-template-columns: 1fr;
    gap: @fun.space.gap.value;
    max-width: 1000px;
    margin: 0 auto;
    padding: 3rem 1.5rem;
    background: @fun.color.bg.value;
  }

  @media (min-width: 800px) {
    .tiers { grid-template-columns: repeat(3, 1fr); }
    .plan-pro { transform: scale(1.04); }
  }
  `
}

@tiers()
```

## The HTML

```html
<section class="tiers">
  <article class="plan-starter">
    <h2>Starter</h2>
    <p class="price">0<span>/mo</span></p>
    <ul><li>1 project</li><li>Community support</li></ul>
    <button class="cta">Choose</button>
  </article>
  <article class="plan-pro">
    <h2>Pro</h2>
    <p class="price">29<span>/mo</span></p>
    <ul><li>Unlimited projects</li><li>Priority support</li></ul>
    <button class="cta">Choose</button>
  </article>
  <article class="plan-studio">
    <h2>Studio</h2>
    <p class="price">99<span>/mo</span></p>
    <ul><li>Everything in Pro</li><li>SSO and audit logs</li></ul>
    <button class="cta">Choose</button>
  </article>
</section>
```

## What Happens

- `@plan()` holds the shared card contract; the generated `.plan-*` rules only add the per-tier accent.
- `@event.emphasis(plan)` returns the accent color for `pro` and `transparent` for everything else, so the emphasis is data, not a hand-written modifier.
- `featured` is a parallel array — it exists so the "which one is highlighted" decision sits next to the plan names, where you can see it.
- The price is a number in an array; the currency mark is a pseudo-element, keeping the value and the symbol separate.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a monthly/yearly toggle with `@event` switching the unit.
2. Compute the yearly discount with `num(<price * 12> * 0.8)`.
3. Give the popular tier a gradient border via `border-image`.

## Summary

Parallel arrays keep the tier data aligned, one define keeps the card consistent, and an event decides emphasis. The pricing table becomes a table of data with styling on top.

---

**Next: [23 — Newsletter Signup](../23-newsletter-signup.md)**
