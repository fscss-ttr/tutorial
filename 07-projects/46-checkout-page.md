# 46 — Checkout Page

A checkout page stitches forms, order summaries, and totals together under pressure. This component demonstrates how arrays, `@event` logic, and token roots coordinate pricing math with a clean two-column layout.

## The Brief

- Build a two-column checkout: form (contact/shipping/payment) and a sticky order summary.
- Drive line items and totals from data arrays (no hardcoded duplication).
- Compute subtotal, tax, and grand total using `num()` with clear units.
- Create a responsive layout that stacks on narrow screens.
- Provide consistent spacing, surfaces, and focus states from tokens.

## The FSCSS

```fscss
/* Design tokens */
@define theme-root(root: :root){
  `
  @use(root){
    --bg: #f8fafc;
    --surface: #ffffff;
    --surface-alt: #f1f5f9;
    --ink: #0f172a;
    --muted: #64748b;
    --border: rgba(15, 23, 42, 0.08);
    --primary: #2563eb;
    --primary-strong: #1d4ed8;
    --ring: rgba(37, 99, 235, 0.24);
    --radius: 12px;
    --shadow: 0 10px 30px rgba(15, 23, 42, 0.08);
  }
  `
}
@theme-root()

/* Palette + space stores */
@fun(color){
  bg: var(--bg, #f8fafc);
  surface: var(--surface, #ffffff);
  surface-alt: var(--surface-alt, #f1f5f9);
  ink: var(--ink, #0f172a);
  muted: var(--muted, #64748b);
  border: var(--border, rgba(15,23,42,0.08));
  primary: var(--primary, #2563eb);
  primary-strong: var(--primary-strong, #1d4ed8);
  ring: var(--ring, rgba(37,99,235,0.24));
}

@fun(space){
  xs: 0.5rem;
  sm: 0.75rem;
  md: 1rem;
  lg: 1.5rem;
  xl: 2rem;
}

@fun(radius){
  md: var(--radius, 12px);
}

@fun(shadow){
  md: var(--shadow, 0 10px 30px rgba(15,23,42,0.08));
}

/* Pricing data */
@arr(itemName[Starter Pack, Pro License, Support Addon])
@arr(itemQty[1, 1, 1])
@arr(itemPrice[2900, 7900, 1500]) /* cents for clarity */

@arr(i[count(@arr.itemPrice!.length, 1)])

/* Tax logic (stateful threshold) */
@event taxRate(region: usd){
  if region == "usd" { return: 0.08; }
  el-if region == "ca" { return: 0.13; }
  el-if region == "eu" { return: 0.20; }
  el { return: 0.10; }
}

/* Layout block */
@define checkout-layout(){
  `
  .checkout {
    background: @fun.color.bg.value;
    color: @fun.color.ink.value;
    font-family: system-ui, -apple-system, sans-serif;
    padding: @fun.space.xl.value 1rem;
  }

  .checkout-inner {
    max-width: 1080px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: minmax(0, 1.5fr) minmax(320px, 1fr);
    gap: @fun.space.xl.value;
    align-items: start;
  }

  .checkout-section {
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: @fun.radius.md.value;
    box-shadow: @fun.shadow.md.value;
    padding: @fun.space.xl.value;
  }

  .checkout-section + .checkout-section {
    margin-top: @fun.space.lg.value;
  }

  .checkout-title {
    font-size: 1.25rem;
    font-weight: 700;
    margin-bottom: @fun.space.lg.value;
  }

  .checkout-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: @fun.space.md.value;
  }

  .checkout-field {
    display: flex;
    flex-direction: column;
    gap: 0.375rem;
  }

  .checkout-field--full {
    grid-column: 1 / -1;
  }

  .checkout-label {
    font-size: 0.9rem;
    font-weight: 600;
  }

  .checkout-input {
    border: 1px solid @fun.color.border.value;
    border-radius: 10px;
    padding: 0.625rem 0.75rem;
    font: inherit;
    background: @fun.color.surface.value;
    color: @fun.color.ink.value;
  }

  .checkout-input:focus {
    outline: 3px solid @fun.color.ring.value;
    outline-offset: 1px;
  }

  .checkout-summary {
    position: sticky;
    top: @fun.space.lg.value;
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: @fun.radius.md.value;
    box-shadow: @fun.shadow.md.value;
    padding: @fun.space.lg.value;
  }

  .summary-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: @fun.space.sm.value 0;
  }

  .summary-meta {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
  }

  .summary-name {
    font-weight: 600;
  }

  .summary-qty {
    color: @fun.color.muted.value;
    font-size: 0.9rem;
  }

  .summary-price {
    font-weight: 600;
  }

  .summary-divider {
    height: 1px;
    background: @fun.color.border.value;
    margin: @fun.space.md.value 0;
  }

  .summary-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.375rem 0;
    color: @fun.color.muted.value;
  }

  .summary-total {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 1.15rem;
    font-weight: 700;
    margin: @fun.space.md.value 0 @fun.space.lg.value;
  }

  .checkout-submit {
    width: 100%;
    border: none;
    border-radius: 999px;
    padding: 0.875rem 1rem;
    font: inherit;
    font-weight: 700;
    background: @fun.color.primary.value;
    color: @fun.color.surface.value;
    cursor: pointer;
    transition: background-color 0.15s ease, transform 0.05s ease;
  }

  .checkout-submit:hover { background: @fun.color.primary-strong.value; }
  .checkout-submit:active { transform: translateY(1px); }
  .checkout-submit:focus { outline: 3px solid @fun.color.ring.value; outline-offset: 2px; }

  .checkout-note {
    color: @fun.color.muted.value;
    font-size: 0.92rem;
    text-align: center;
    margin-top: @fun.space.sm.value;
  }

  @media (max-width: 920px) {
    .checkout-inner {
      grid-template-columns: 1fr;
    }
    .checkout-grid {
      grid-template-columns: 1fr;
    }
  }
  `
}
@checkout-layout()
```

## The Data

```fscss
/* Subtotal (cents) */
$subtotalCents: 0;

/* Build subtotal by walking items */
@arr _c[count(@arr.itemPrice!.length, 1)]
.subtotal-@arr._c[@arr._c[]] {
  $idx: @arr._c[];
  $line: num(@arr.itemPrice[$idx!] * @arr.itemQty[$idx!]);
  $subtotalCents: num($subtotalCents! + $line);
}

/* Totals */
$taxCents: num($subtotalCents! * @event.taxRate("usd"));
$totalCents: num($subtotalCents! + $taxCents!);

/* Format helpers (read-only values) */
$subtotalFmt: $subtotalCents! / 100;
$taxFmt: $taxCents! / 100;
$totalFmt: $totalCents! / 100;
```

## The HTML

```html
<main class="checkout">
  <div class="checkout-inner">
    <section>
      <form class="checkout-section">
        <h2 class="checkout-title">Contact</h2>
        <div class="checkout-grid">
          <div class="checkout-field checkout-field--full">
            <label class="checkout-label" for="email">Email</label>
            <input id="email" class="checkout-input" type="email" placeholder="you@example.com">
          </div>
          <div class="checkout-field checkout-field--full">
            <label class="checkout-label" for="phone">Phone</label>
            <input id="phone" class="checkout-input" type="tel" placeholder="(555) 123-4567">
          </div>
        </div>
      </form>

      <form class="checkout-section">
        <h2 class="checkout-title">Shipping</h2>
        <div class="checkout-grid">
          <div class="checkout-field">
            <label class="checkout-label" for="first">First name</label>
            <input id="first" class="checkout-input" type="text" placeholder="Alex">
          </div>
          <div class="checkout-field">
            <label class="checkout-label" for="last">Last name</label>
            <input id="last" class="checkout-input" type="text" placeholder="River">
          </div>
          <div class="checkout-field checkout-field--full">
            <label class="checkout-label" for="addr">Address</label>
            <input id="addr" class="checkout-input" type="text" placeholder="123 Market St">
          </div>
          <div class="checkout-field">
            <label class="checkout-label" for="city">City</label>
            <input id="city" class="checkout-input" type="text" placeholder="San Francisco">
          </div>
          <div class="checkout-field">
            <label class="checkout-label" for="zip">ZIP</label>
            <input id="zip" class="checkout-input" type="text" placeholder="94105">
          </div>
        </div>
      </form>

      <form class="checkout-section">
        <h2 class="checkout-title">Payment</h2>
        <div class="checkout-grid">
          <div class="checkout-field checkout-field--full">
            <label class="checkout-label" for="card">Card number</label>
            <input id="card" class="checkout-input" type="text" placeholder="4242 4242 4242 4242">
          </div>
          <div class="checkout-field">
            <label class="checkout-label" for="exp">Expiry</label>
            <input id="exp" class="checkout-input" type="text" placeholder="MM/YY">
          </div>
          <div class="checkout-field">
            <label class="checkout-label" for="cvc">CVC</label>
            <input id="cvc" class="checkout-input" type="text" placeholder="123">
          </div>
        </div>
      </form>
    </section>

    <aside class="checkout-summary">
      <h2 class="checkout-title">Order Summary</h2>

      <div class="summary-item">
        <div class="summary-meta">
          <div class="summary-name">Starter Pack</div>
          <div class="summary-qty">Qty 1</div>
        </div>
        <div class="summary-price">$29.00</div>
      </div>

      <div class="summary-item">
        <div class="summary-meta">
          <div class="summary-name">Pro License</div>
          <div class="summary-qty">Qty 1</div>
        </div>
        <div class="summary-price">$79.00</div>
      </div>

      <div class="summary-item">
        <div class="summary-meta">
          <div class="summary-name">Support Addon</div>
          <div class="summary-qty">Qty 1</div>
        </div>
        <div class="summary-price">$15.00</div>
      </div>

      <div class="summary-divider"></div>

      <div class="summary-row">
        <span>Subtotal</span>
        <span>$123.00</span>
      </div>
      <div class="summary-row">
        <span>Tax (8%)</span>
        <span>$9.84</span>
      </div>

      <div class="summary-total">
        <span>Total</span>
        <span>$132.84</span>
      </div>

      <button class="checkout-submit">Complete Purchase</button>
      <p class="checkout-note">Secure checkout • SSL encrypted</p>
    </aside>
  </div>
</main>
```

## What Happens

- Token root `@theme-root()` emits a single `:root` with all CSS custom properties; `@fun` reads them with safe fallbacks.
- Arrays hold line items; `count(@arr.itemPrice!.length)` drives a small loop that accumulates `$subtotalCents` via `num()` (integer cents avoid float drift).
- `@event.taxRate(region)` encapsulates regional tax as a pure switch; called with `"usd"` to produce `$taxCents` and `$totalCents`.
- `@checkout-layout()` is a block define that emits the full layout, sections, form controls, sticky summary, and a `max-width: 920px` breakpoint.
- Form fields share focus ring via `outline: 3px solid @fun.color.ring.value` and consistent border radius for accessibility.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Make region selectable (e.g. USD/CA/EU) and recompute totals via `@event.taxRate` for the chosen value.
2. Add quantity inputs that multiply `itemQty` in the loop and recalc totals.
3. Generate the summary list from `@arr` (name/qty/price/line total) instead of hardcoding HTML rows.

## Summary

Arrays + `num()` handle pricing math, `@event` centralizes business rules, and a token root + block define keeps the checkout's layout and states consistent without duplicating styles.

---

**Next: [47 — Blog Layout](../47-blog-layout.md)**
