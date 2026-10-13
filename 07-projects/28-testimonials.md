# 28 — Testimonials

Testimonials repeat the same card shape with different copy. Drive them with arrays and a counter loop, style from tokens, and keep quotes/avatars in a compact `@define`.

## The Brief

- A `.testimonials` grid of `.testimonial` cards.
- Author name, role, quote, and avatar initial.
- Generated styling/structure driven by arrays (use `count()` to walk items).
- Subtle quotation mark, consistent radius/shadow, and a responsive grid.

## The FSCSS

```fscss
@fun(color){
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  accent: #2563eb;
  border: rgba(15, 23, 42, 0.06);
  bg: #f8fafc;
}

@fun(space){
  pad: 1.75rem;
  gap: 1.25rem;
}

@define testimonials-root() {
  `
  :root {
    --quote-size: 2.25rem;
  }
  `
}

@define testimonials-grid() {
  `
  .testimonials {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: @fun.space.gap.value;
    padding: 2rem clamp(1rem, 5vw, 3rem);
    background: @fun.color.bg.value;
  }

  @media (max-width: 1080px) {
    .testimonials { grid-template-columns: repeat(2, minmax(0, 1fr)); }
  }

  @media (max-width: 680px) {
    .testimonials { grid-template-columns: 1fr; }
  }
  `
}

@define testimonial-card() {
  `
  .testimonial {
    position: relative;
    background: @fun.color.surface.value;
    border-radius: 16px;
    padding: @fun.space.pad.value;
    box-shadow: 0 10px 40px rgba(15, 23, 42, 0.06);
    border: 1px solid @fun.color.border.value;
    display: flex;
    flex-direction: column;
    gap: 1.25rem;
  }

  .testimonial::before {
    content: "“";
    position: absolute;
    top: -0.25rem;
    left: 1rem;
    font-size: var(--quote-size, 2.25rem);
    line-height: 1;
    color: @fun.color.accent.value;
    opacity: 0.16;
  }

  .testimonial-quote {
    color: @fun.color.ink.value;
    line-height: 1.8;
    font-size: 0.98rem;
    margin: 0;
    z-index: 1;
  }

  .testimonial-meta {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    margin-top: auto;
  }

  .testimonial-avatar {
    width: 42px;
    height: 42px;
    border-radius: 999px;
    background: rgba(37, 99, 235, 0.14);
    color: @fun.color.accent.value;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    font-weight: 700;
    letter-spacing: 0.02em;
  }

  .testimonial-name {
    color: @fun.color.ink.value;
    font-weight: 600;
  }

  .testimonial-role {
    color: @fun.color.muted.value;
    font-size: 0.88rem;
  }
  `
}

@testimonials-root()
@testimonials-grid()
@testimonial-card()
```

## The Data

```fscss
@arr(name[Mara, Nico, Lin])
@arr(role[Staff Eng, UX Lead, PM])
@arr(quote[
  "FSCSS cut our CSS diff in half without losing readability.",
  "The define+array combo is the first thing I reach for now.",
  "Tokens + loops make onboarding to a new page trivial."
])
@arr(initial[M, N, L])
@arr(i[count(@arr.name!.length, 1)])
```

## The HTML

```html
<section class="testimonials">
  <article class="testimonial">
    <p class="testimonial-quote">FSCSS cut our CSS diff in half without losing readability.</p>
    <div class="testimonial-meta">
      <span class="testimonial-avatar">M</span>
      <div>
        <div class="testimonial-name">Mara</div>
        <div class="testimonial-role">Staff Eng</div>
      </div>
    </div>
  </article>
  <article class="testimonial">
    <p class="testimonial-quote">The define+array combo is the first thing I reach for now.</p>
    <div class="testimonial-meta">
      <span class="testimonial-avatar">N</span>
      <div>
        <div class="testimonial-name">Nico</div>
        <div class="testimonial-role">UX Lead</div>
      </div>
    </div>
  </article>
  <article class="testimonial">
    <p class="testimonial-quote">Tokens + loops make onboarding to a new page trivial.</p>
    <div class="testimonial-meta">
      <span class="testimonial-avatar">L</span>
      <div>
        <div class="testimonial-name">Lin</div>
        <div class="testimonial-role">PM</div>
      </div>
    </div>
  </article>
</section>
```

## What Happens

- The array set (`name`, `role`, `quote`, `initial`, `i`) is the data source; with `count(@arr.name!.length,1)` the index walks all three cleanly.
- `@testimonial-card()` centralizes the quote decoration (`::before`), avatar, and meta layout; changing padding/shadow once updates every card.
- Breakpoints live inside `testimonials-grid()` so the responsive behaviour is right next to the grid definition.
- Tokens keep surface, ink, muted, and accent consistent across the whole section.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Use the `i` array to conceptually bind avatar initial, name, role, quote (CSS-only mapping via the data you already have).
2. Add a `.testimonial--featured` variant with a larger shadow and an accent border.
3. Randomize a subtle background accent per card using `@random` (kept within the card token space).

## Summary

Data as arrays + index + one card define = a testimonials block that scales to ten items with zero style duplication, and still reads like plain CSS.

---

**Next: [29 — Dashboard Layout](../29-dashboard-layout.md)**