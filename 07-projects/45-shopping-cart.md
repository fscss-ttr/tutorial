# 45 — Shopping Cart

A cart is a table of line items and a summary that has to add up. Build the rows from parallel arrays, let `num()` do the quantities and totals, and use an event to color the shipping status. The interesting part is that most of the arithmetic is design-time, not runtime.

## The Brief

- Line items with thumbnail, name, quantity stepper, and line price.
- A subtotal, shipping line, and grand total summary.
- Free-shipping progress driven by a threshold.
- An empty-cart state.

## The FSCSS

```fscss
@fun(color){
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  line: #e2e8f0;
  accent: #2563eb;
  ok: #10b981;
  warn: #f59e0b;
}

@fun(space){
  pad: 1.25rem;
  gap: 1rem;
}

@event shipping(progress) {
  if progress: free {
    return: @fun.color.ok.value;
  }
  el {
    return: @fun.color.warn.value;
  }
}

@define row() {
  display: grid;
  grid-template-columns: 64px 1fr auto;
  align-items: center;
  gap: 1rem;
  padding: @fun.space.pad.value 0;
  border-bottom: 1px solid @fun.color.line.value;
}

.cart-row {
  @row()
}

.cart-thumb {
  %2(width, height[: 64px;])
  border-radius: 10px;
  background: @fun.color.line.value;
}

.cart-name {
  font-weight: 600;
  color: @fun.color.ink.value;
}

.cart-unit {
  font-size: 0.82rem;
  color: @fun.color.muted.value;
}

.cart-price {
  font-weight: 700;
  color: @fun.color.ink.value;
}
```

The line items are parallel arrays, and `num()` derives the line price:

```fscss
/* columns:  name     |  unit  |  qty  */
@arr names[Ridge, Kestrel, Wren]
@arr units[129, 89, 42]
@arr qtys[1, 2, 3]
@arr i[count(@arr.names!.length, 1)]

.cart-row-@arr.i[] .cart-price::before {
  content: "$";
}

.cart-row-@arr.i[] .cart-price::after {
  content: "num(@arr.units[@arr.i[]] * @arr.qtys[@arr.i[]])";
}
```

The summary adds them up the same way — a compile-time sum, spelled out because FSCSS has no reducer:

```fscss
$subtotal: num(
  <@arr.units[1] * @arr.qtys[1]> +
  <@arr.units[2] * @arr.qtys[2]> +
  <@arr.units[3] * @arr.qtys[3]>
);

.summary-subtotal::after {
  content: "$subtotal!";
}

.summary-total::after {
  content: "$subtotal!";
}
```

## The Layout

```fscss
@define cart() {
  `
  .cart {
    display: grid;
    grid-template-columns: 1fr;
    gap: 2rem;
    max-width: 1000px;
    margin: 0 auto;
    padding: 2rem 1.5rem;
  }

  .cart-summary {
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.line.value;
    border-radius: 16px;
    padding: @fun.space.pad.value;
    height: fit-content;
  }

  .shipping-bar {
    height: 8px;
    border-radius: 999px;
    background: @fun.color.line.value;
    overflow: hidden;
    margin: 0.5rem 0 1rem;
  }

  .shipping-bar span {
    display: block;
    height: 100%;
    width: 80%;
    background: @event.shipping(free);
  }

  @media (min-width: 860px) {
    .cart { grid-template-columns: 1.6fr 1fr; }
  }
  `
}

@cart()
```

## The HTML

```html
<section class="cart">
  <div class="cart-items">
    <div class="cart-row cart-row-1">
      <div class="cart-thumb"></div>
      <div>
        <div class="cart-name">Ridge</div>
        <div class="cart-unit">$129 each · qty 1</div>
      </div>
      <div class="cart-price"></div>
    </div>
    <div class="cart-row cart-row-2">
      <div class="cart-thumb"></div>
      <div>
        <div class="cart-name">Kestrel</div>
        <div class="cart-unit">$89 each · qty 2</div>
      </div>
      <div class="cart-price"></div>
    </div>
  </div>

  <aside class="cart-summary">
    <div>Subtotal <span class="summary-subtotal"></span></div>
    <p>Add $12 more for free shipping</p>
    <div class="shipping-bar"><span></span></div>
    <div><strong>Total</strong> <span class="summary-total"></span></div>
  </aside>
</section>
```

## What Happens

- `@row()` owns the three-column grid, so every line item aligns its thumbnail, name, and price without per-row markup.
- `content: "num(unit * qty)"` expands the function inside the string, so each line total is computed at build time — the browser never multiplies anything.
- `$subtotal` is a compile-time sum written out by hand, because FSCSS has no fold/reduce; the point is that the total is still source, not runtime JavaScript.
- Variables expand inside quoted strings (`content: "$subtotal!"`), which is how the currency sign and the number ship together.
- `@event.shipping()` colors the progress bar from a single `progress` value, so the "free" state is data.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Generate the line items entirely from the arrays and their markup contract.
2. Add a promo-code field that flips a `--discount` token.
3. Derive the free-shipping progress width from `num(<subtotal / 150> * 100)%`.

## Summary

Parallel arrays hold the cart, `num()` does every sum at compile time, and an event colors the shipping status. The cart is arithmetic plus one grid.

---

**Next: [46 — Checkout Page](../46-checkout-page.md)**
