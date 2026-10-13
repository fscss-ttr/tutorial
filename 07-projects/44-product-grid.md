# 44 — Product Grid

An e-commerce grid has to survive variety: different prices, some items on sale, some out of stock, all sharing one card. Build the card once, drive the badges and states from an event, and let the grid do the responsiveness.

## The Brief

- A responsive product grid, two columns on phones.
- Each card: image, name, price, and a status badge.
- Badge variants — new, sale, sold out — from an event.
- A muted treatment for unavailable products.

## The FSCSS

```fscss
@fun(color){
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  sale: #ef4444;
  new: #2563eb;
  sold: #94a3b8;
  line: #e2e8f0;
}

@fun(space){
  pad: 1rem;
  gap: 1rem;
}

@event badge(status) {
  if status: sale {
    return: @fun.color.sale.value;
  }
  if status: new {
    return: @fun.color.new.value;
  }
  if status: sold {
    return: @fun.color.sold.value;
  }
  el {
    return: transparent;
  }
}

@define product(radius: 14px) {
  position: relative;
  background: @fun.color.surface.value;
  border: 1px solid @fun.color.line.value;
  border-radius: @use(radius);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: box-shadow 0.2s ease;
}

@define thumb() {
  aspect-ratio: 4 / 3;
  background: @fun.color.line.value;
  position: relative;
}

.product-thumb {
  @thumb()
}

.product-name {
  font-weight: 600;
  color: @fun.color.ink.value;
  padding: 0.75rem @fun.space.pad.value 0;
}

.product-price {
  color: @fun.color.muted.value;
  padding: 0.15rem @fun.space.pad.value 1rem;
}
```

The products are parallel arrays — name, price, status, and an old price for sales:

```fscss
/* columns:  name     |  price  |  status  |  was   */
@arr names[Ridge, Kestrel, Onyx, Dune, Vela, Wren]
@arr prices[129, 89, 240, 65, 158, 42]
@arr states[new, sale, none, sale, sold, new]
@arr was[0, 129, 0, 89, 0, 0]
@arr i[count(@arr.names!.length, 1)]

@define grid() {
  `
  .products {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: @fun.space.gap.value;
    padding: 2rem 1.5rem;
    max-width: 1100px;
    margin: 0 auto;
  }

  .product { @product(); }

  .product:hover { box-shadow: 0 12px 28px rgba(15, 23, 42, 0.12); }

  .product-@arr.i[]::before {
    content: "@arr.states[@arr.i[]]";
    position: absolute;
    margin: 0.6rem;
    padding: 0.2rem 0.6rem;
    border-radius: 999px;
    font-size: 0.68rem;
    font-weight: 700;
    text-transform: uppercase;
    color: white;
    background: @event.badge(@arr.states[@arr.i[]]);
  }

  .product.sold {
    opacity: 0.55;
  }

  @media (min-width: 768px) {
    .products { grid-template-columns: repeat(3, 1fr); }
  }

  @media (min-width: 1100px) {
    .products { grid-template-columns: repeat(4, 1fr); }
  }
  `
}

@grid()
```

The sale strike-through only exists where there is an old price:

```fscss
.product-@arr.i[] .product-price {
  font-weight: 700;
  color: @fun.color.ink.value;
}

.product-@arr.i[] .was {
  text-decoration: line-through;
  color: @fun.color.muted.value;
  margin-left: 0.4rem;
  font-weight: 400;
}
```

## The HTML

```html
<section class="products">
  <article class="product product-1">
    <div class="product-thumb"></div>
    <h3 class="product-name">Ridge</h3>
    <div class="product-price">$129</div>
  </article>
  <article class="product product-2">
    <div class="product-thumb"></div>
    <h3 class="product-name">Kestrel</h3>
    <div class="product-price">$89 <span class="was">$129</span></div>
  </article>
  <article class="product product-5 sold">
    <div class="product-thumb"></div>
    <h3 class="product-name">Vela</h3>
    <div class="product-price">$158</div>
  </article>
</section>
```

## What Happens

- `@event.badge()` maps a status to a color, and the generated `product-*::before` rules read both the label and the color from the status array — one column of data paints the badge.
- The `states` array uses `none` as a real value, and `@event.badge` falls through to `transparent`, which is how a product with no badge still compiles.
- `sold` gets `opacity: 0.55` from a hand-written class because it's a state of the whole card, not something the loop should infer.
- The grid is a block define with three breakpoints, so the column count and its media queries never drift apart.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Generate the sale badge's old price from `num(<price * 1.45>)` instead of a separate array.
2. Add "quick add" button with a hover reveal using a block define.
3. Make the sold-out card non-interactive with `pointer-events: none`.

## Summary

One card define, one grid define, and four parallel arrays. Every badge, price, and state comes from data — the markup only names the product.

---

**Next: [45 — Shopping Cart](../45-shopping-cart.md)**
