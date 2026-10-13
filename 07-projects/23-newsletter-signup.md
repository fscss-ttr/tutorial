# 23 — Newsletter Signup

A newsletter bar is one row with three jobs: say something worth subscribing to, take an email, and reassure. Build the input and button from shared defines, and let an event carry the success and error states so the markup stays clean.

## The Brief

- A full-width band with copy on the left and the form on the right.
- Input and button share height, radius, and font.
- Success and error states driven by an event.
- Collapses to a stacked layout on mobile.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #475569;
  band: #0b1220;
  surface: #ffffff;
  accent: #2563eb;
  ok: #10b981;
  err: #ef4444;
}

@fun(radius){
  field: 10px;
  pill: 999px;
}

@define field(height: 46px, radius: 10px) {
  height: @use(height);
  border-radius: @use(radius);
  border: 1px solid rgba(255, 255, 255, 0.18);
  background: rgba(255, 255, 255, 0.06);
  color: @fun.color.surface.value;
  padding: 0 1rem;
  font-size: 0.95rem;
  outline: none;
}

@define cta(height: 46px, radius: 10px) {
  height: @use(height);
  border: none;
  border-radius: @use(radius);
  background: @fun.color.accent.value;
  color: white;
  font-weight: 600;
  padding: 0 1.5rem;
  cursor: pointer;
  transition: filter 0.15s ease;
}

.newsletter-btn {
  @cta()
}

.newsletter-btn:hover {
  filter: brightness(1.08);
}
```

The state lives in an event, so one `data-state` attribute drives the message color:

```fscss
@event status(state) {
  if state: ok {
    return: @fun.color.ok.value;
  }
  if state: err {
    return: @fun.color.err.value;
  }
  el {
    return: @fun.color.muted.value;
  }
}

.newsletter-note {
  color: @event.status(ok);
  font-size: 0.82rem;
}
```

## The Layout

```fscss
@define band() {
  `
  .newsletter {
    background: @fun.color.band.value;
    color: @fun.color.surface.value;
    padding: 3rem 1.5rem;
  }

  .newsletter-inner {
    display: grid;
    gap: 1.5rem;
    max-width: 1000px;
    margin: 0 auto;
    align-items: center;
  }

  .newsletter-form {
    display: flex;
    gap: 0.75rem;
  }

  .newsletter-form input {
    @field()
    flex: 1;
  }

  @media (min-width: 760px) {
    .newsletter-inner { grid-template-columns: 1.1fr 1fr; }
  }

  @media (max-width: 480px) {
    .newsletter-form { flex-direction: column; }
  }
  `
}

@band()
```

## The HTML

```html
<section class="newsletter">
  <div class="newsletter-inner">
    <div>
      <h2>Ship faster with FSCSS</h2>
      <p>One short email, once a month. Unsubscribe anytime.</p>
    </div>
    <form class="newsletter-form" data-state="ok">
      <input type="email" placeholder="you@example.com" aria-label="Email">
      <button class="newsletter-btn" type="submit">Subscribe</button>
    </form>
    <p class="newsletter-note">You're on the list. See you in March.</p>
  </div>
</section>
```

## What Happens

- `@field()` and `@cta()` share `height` and `radius` parameters, so the input and button line up without a magic number in two places.
- `@event.status()` turns the form's state into a color, so changing `data-state` changes the note without new rules.
- The band is a block define, which is why the mobile stacking rules sit beside the desktop grid.
- The curved radius option (`999px`) is in the `@fun(radius)` store, ready for a pill-shaped variant.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add an inline error message that swaps text via `data-state`.
2. Animate the submit button into a spinner with a keyframes block define.
3. Add a checkbox for consent and style it with the same field tokens.

## Summary

Two defines keep the form controls honest, one event carries state, and one block define owns the responsive band. Small component, all the patterns.

---

**Next: [24 — Timeline](../24-timeline.md)**
