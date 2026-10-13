# 43 — Landing Page

This is the capstone for everything so far: a full landing page assembled from a token root, a generated utility layer, section block defines, and events for the theme and the CTA. Read it as a checklist — each FSCSS feature earns its place.

## The Brief

- Hero, feature grid, and call-to-action band.
- Tokens published once to `:root`, consumed everywhere.
- A generated spacing utility layer.
- Dark mode via a data attribute and an event.

## The FSCSS — Tokens

Published with a `*-root` define so every section reads from one place:

```fscss
@fun(palette){
  ink: #0f172a;
  surface: #ffffff;
  muted: #64748b;
  accent: #2563eb;
  bg: #f8fafc;
}

@define landing-tokens(root: :root) {
  `
  @use(root) {
    --bg: @fun.palette.bg.value;
    --surface: @fun.palette.surface.value;
    --ink: @fun.palette.ink.value;
    --muted: @fun.palette.muted.value;
    --accent: @fun.palette.accent.value;
    --radius: 16px;
    --gutter: 1.5rem;
  }
  `
}

@landing-tokens()
```

The dark theme is another root define, scoped to an attribute:

```fscss
@define landing-dark(root: [data-theme="dark"]) {
  `
  @use(root) {
    --bg: #0b1220;
    --surface: #111a2e;
    --ink: #e2e8f0;
    --muted: #94a3b8;
    --accent: #60a5fa;
  }
  `
}

@landing-dark()
```

## The FSCSS — Generated Utilities

A small spacing scale, generated once:

```fscss
@arr steps[1, 2, 3, 4, 5, 6]
@arr si[count(6, 1)]

.stack-@arr.steps[@arr.si[]] > * + * {
  margin-top: num(@arr.steps[@arr.si[]] * 0.5)rem;
}
```

An event reads the theme to flip the CTA:

```fscss
@event cta(theme) {
  if theme: dark {
    return: #60a5fa;
  }
  el {
    return: #2563eb;
  }
}

[data-theme="dark"] .cta {
  background: @event.cta(dark);
}
```

## The FSCSS — Sections

Each section is a block define, so its rules and breakpoints travel together:

```fscss
@define hero() {
  `
  .hero {
    background: var(--bg, #f8fafc);
    color: var(--ink, #0f172a);
    padding: 6rem var(--gutter);
    text-align: center;
  }

  .hero h1 {
    font-size: clamp(2rem, 5vw, 3.25rem);
    line-height: 1.1;
    letter-spacing: -0.02em;
    margin-bottom: 1rem;
  }

  .hero p {
    color: var(--muted, #64748b);
    max-width: 540px;
    margin: 0 auto 2rem;
  }

  .cta {
    display: inline-block;
    background: var(--accent, #2563eb);
    color: white;
    font-weight: 600;
    padding: 0.85rem 1.75rem;
    border-radius: 999px;
    text-decoration: none;
    transition: transform 0.15s ease;
  }

  .cta:hover { transform: translateY(-2px); }
  `
}

@define features() {
  `
  .features {
    display: grid;
    grid-template-columns: 1fr;
    gap: var(--gutter);
    padding: 4rem var(--gutter);
    max-width: 1080px;
    margin: 0 auto;
  }

  .feature {
    background: var(--surface, #fff);
    border-radius: var(--radius, 16px);
    padding: 1.5rem;
  }

  @media (min-width: 768px) {
    .features { grid-template-columns: repeat(3, 1fr); }
  }
  `
}

@hero()
@features()
```

## The HTML

```html
<html data-theme="light">
<body>
  <section class="hero">
    <h1>Styling, at the speed of thought</h1>
    <p>FSCSS turns repetitive CSS into a handful of tokens and one loop.</p>
    <a class="cta" href="#start">Get started</a>
  </section>

  <section class="features stack-4">
    <article class="feature"><h3>Tokens</h3><p>One palette, every surface.</p></article>
    <article class="feature"><h3>Loops</h3><p>Data in, utilities out.</p></article>
    <article class="feature"><h3>Themes</h3><p>Dark mode from one attribute.</p></article>
  </section>
</body>
</html>
```

## What Happens

- `@landing-tokens()` publishes the whole palette to `:root`, and every section reads `var(--x, fallback)` — nothing downstream hardcodes a color.
- `@landing-dark()` is the same token contract scoped to `[data-theme="dark"]`, so flipping one attribute re-themes the page without touching a rule.
- The `stack-*` utilities are generated from `steps` + `count()`, which is how a handful of lines produce a whole spacing scale.
- `@event.cta(theme)` handles the one case where the theme needs a value the CSS variables don't already carry (a lighter accent on dark).

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a `prefers-color-scheme` fallback block for `@landing-dark()`.
2. Generate the feature grid columns from a `@arr.cols` array.
3. Add a sticky nav whose background switches via `@event` on scroll state.

## Summary

Tokens published once, utilities generated from arrays, sections in block defines, and an event for the exception. This is the full stack — the same recipe every project before it has been building toward.

---

**Next: [44 — Product Grid](../44-product-grid.md)**
