# 02 — Card Component

A card is the most reused surface on the web. Build one with a header, body, footer, a hover raise, and a token-driven radius — and prove a single `@define` can dress both a product card and a profile card.

## The Brief

- A `card` define handling surface, radius, padding, and shadow.
- `.card-header`, `.card-body`, `.card-footer` inside.
- A `.card--hover` modifier that lifts on hover.
- Used twice in the page with different sizes.

## The FSCSS

```fscss
@fun(color){
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  bg: #f1f5f9;
}

@fun(space){ md: 1.5rem; }

@define card(pad: 1.5rem, radius: 14px, shadow: 0 6px 20px rgba(15,23,42,0.08)) {
  background: @use(@fun.color.surface.value);
  border-radius: @use(radius);
  padding: @use(pad);
  box-shadow: @use(shadow);
}
```

Careful: a define parameter can't hold `@fun` output in a call like that. The readable form passes values straight through:

```fscss
@define card(pad: 1.5rem, radius: 14px) {
  background: #ffffff;
  border-radius: @use(radius);
  padding: @use(pad);
  box-shadow: 0 6px 20px rgba(15, 23, 42, 0.08);
  border: 1px solid rgba(15, 23, 42, 0.06);
}

.card {
  @card()
}

.card--hover {
  @card(1.5rem, 16px)
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.card--hover:hover {
  transform: translateY(-4px);
  box-shadow: 0 16px 32px rgba(15, 23, 42, 0.12);
}

.card-header {
  font-weight: 700;
  color: @fun.color.ink.value;
  margin-bottom: 0.5rem;
}

.card-body {
  color: @fun.color.muted.value;
  line-height: 1.55;
}

.card-footer {
  margin-top: 1rem;
  padding-top: 0.75rem;
  border-top: 1px solid rgba(15, 23, 42, 0.08);
}

body {
  background: @fun.color.bg.value;
  font-family: system-ui, sans-serif;
  display: flex;
  gap: 1.5rem;
  padding: 2rem;
}
```

## The HTML

```html
<article class="card">
  <div class="card-header">Daily summary</div>
  <div class="card-body">Three meetings, two merges, one shipped feature. A calm Tuesday.</div>
  <div class="card-footer">Updated 2 min ago</div>
</article>

<article class="card card--hover">
  <div class="card-header">Weekly digest</div>
  <div class="card-body">A longer view: green builds all week.</div>
</article>
```

## What Happens

- `.card` and `.card--hover` differ by two arguments and a transition — the define keeps the surface logic in one place.
- The hover shadow replaces, not stacks, via an explicit box-shadow (same specificity, later rule wins).
- Radius/padding stay token-flavoured through defaults.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a `--inverse` variant with a dark surface and light text.
2. Give cards a `radius` knob as a CSS `--card-radius` token instead of a define arg.
3. Turn the header/body/footer into `str()` blocks and compose a bare-minimal card.

## Summary

One `@define` + two modifiers + a class contract. Cards multiply, code doesn't.

---

**Next: [03 — Navigation Bar](../03-navigation-bar.md)**