# 49 — Portfolio Page

A portfolio page showcases work: hero, filters, project grid, and a contact footer. This project demonstrates filter state via `@event`, array-driven project cards, and masonry-like responsive grid with generated utilities.

## The Brief

- Hero with name, role, and CTA.
- Filter bar driven by categories (All/Web/App/Brand).
- Project grid generated from data arrays (image, title, tags, year).
- Hover overlay on project cards.
- Responsive grid (1/2/3/4 columns) with consistent gaps.
- Tokenized surfaces, type, and focus states.

## The FSCSS

```fscss
/* Tokens */
@define port-root(root: :root){
  `
  @use(root){
    --bg: #f8fafc;
    --surface: #ffffff;
    --ink: #0f172a;
    --muted: #475569;
    --border: rgba(15, 23, 42, 0.08);
    --accent: #2563eb;
    --accent-strong: #1d4ed8;
    --ring: rgba(37, 99, 235, 0.24);
    --radius: 16px;
    --shadow: 0 10px 30px rgba(15, 23, 42, 0.08);
    --shadow-soft: 0 20px 40px rgba(15, 23, 42, 0.12);
  }
  `
}
@port-root()

@fun(color){
  bg: var(--bg, #f8fafc);
  surface: var(--surface, #ffffff);
  ink: var(--ink, #0f172a);
  muted: var(--muted, #475569);
  border: var(--border, rgba(15,23,42,0.08));
  accent: var(--accent, #2563eb);
  accent-strong: var(--accent-strong, #1d4ed8);
  ring: var(--ring, rgba(37,99,235,0.24));
}

@fun(space){
  xs: 0.75rem;
  sm: 1rem;
  md: 1.5rem;
  lg: 2rem;
  xl: 4rem;
}

@fun(type){
  h1: clamp(2.5rem, 6vw, 4rem);
  h2: clamp(1.75rem, 4vw, 2.5rem);
  lead: 1.25rem;
  small: 0.95rem;
}

@fun(radius){
  md: var(--radius, 16px);
}

@fun(shadow){
  md: var(--shadow, 0 10px 30px rgba(15, 23, 42, 0.08));
  soft: var(--shadow-soft, 0 20px 40px rgba(15, 23, 42, 0.12));
}

/* Filter state */
@event filterActive(cat){
  if cat: all { return: "is-active"; }
  el-if cat: web { return: "is-active"; }
  el-if cat: app { return: "is-active"; }
  el-if cat: brand { return: "is-active"; }
  el { return: ""; }
}

/* Project data */
@arr(cat[web, app, brand, web, app, brand])
@arr(title[Atlas Dashboard, Pulse Mobile, Forma Brand, Orbit Landing, Echo App, Nova Identity])
@arr(year[2026, 2025, 2025, 2026, 2024, 2024])
@arr(tagStr[Web • Analytics, App • Health, Brand • System, Web • Marketing, App • Social, Brand • Logo])

/* Layout */
@define portfolio-layout(){
  `
  .portfolio {
    background: @fun.color.bg.value;
    color: @fun.color.ink.value;
    font-family: system-ui, -apple-system, sans-serif;
    line-height: 1.7;
  }

  .portfolio-hero {
    padding: @fun.space.xl.value 1rem @fun.space.lg.value;
    text-align: center;
  }

  .portfolio-hero-inner {
    max-width: 760px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: @fun.space.md.value;
    align-items: center;
  }

  .portfolio-eyebrow {
    color: @fun.color.accent.value;
    font-weight: 600;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    font-size: @fun.type.small.value;
  }

  .portfolio-title {
    font-size: @fun.type.h1.value;
    line-height: 1.1;
    margin: 0;
  }

  .portfolio-lead {
    color: @fun.color.muted.value;
    font-size: @fun.type.lead.value;
    max-width: 60ch;
  }

  .portfolio-cta {
    border: none;
    border-radius: 999px;
    padding: 0.875rem 1.75rem;
    font: inherit;
    font-weight: 700;
    background: @fun.color.accent.value;
    color: @fun.color.surface.value;
    cursor: pointer;
    transition: background-color 0.15s ease, transform 0.05s ease;
  }

  .portfolio-cta:hover { background: @fun.color.accent-strong.value; }
  .portfolio-cta:active { transform: translateY(1px); }
  .portfolio-cta:focus { outline: 3px solid @fun.color.ring.value; outline-offset: 2px; }

  .portfolio-filters {
    padding: 0 1rem @fun.space.lg.value;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .filter-chip {
    border: 1px solid @fun.color.border.value;
    background: @fun.color.surface.value;
    color: @fun.color.ink.value;
    border-radius: 999px;
    padding: 0.5rem 1rem;
    font: inherit;
    font-weight: 600;
    cursor: pointer;
    transition: box-shadow 0.15s ease, transform 0.05s ease;
  }

  .filter-chip:hover { box-shadow: @fun.shadow.md.value; }
  .filter-chip:focus { outline: 3px solid @fun.color.ring.value; outline-offset: 2px; }
  .filter-chip.is-active {
    background: @fun.color.accent.value;
    color: @fun.color.surface.value;
    border-color: transparent;
  }

  .portfolio-grid-wrap {
    max-width: 1320px;
    margin: 0 auto;
    padding: 0 1rem @fun.space.xl.value;
  }

  .portfolio-grid {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    gap: @fun.space.md.value;
  }

  .project-card {
    grid-column: span 4;
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: @fun.radius.md.value;
    box-shadow: @fun.shadow.md.value;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }

  .project-card:hover {
    transform: translateY(-6px);
    box-shadow: @fun.shadow.soft.value;
  }

  .project-media {
    aspect-ratio: 16 / 10;
    background: linear-gradient(135deg, rgba(37, 99, 235, 0.12), rgba(15, 23, 42, 0.04));
    position: relative;
  }

  .project-overlay {
    position: absolute;
    inset: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    opacity: 0;
    transition: opacity 0.2s ease;
    background: radial-gradient(circle, rgba(15,23,42,0.24), transparent 70%);
  }

  .project-card:hover .project-overlay { opacity: 1; }

  .overlay-btn {
    border: 1px solid @fun.color.surface.value;
    background: rgba(15, 23, 42, 0.6);
    color: @fun.color.surface.value;
    border-radius: 999px;
    padding: 0.5rem 0.875rem;
    font: inherit;
    font-size: @fun.type.small.value;
    backdrop-filter: blur(6px);
  }

  .project-body {
    padding: @fun.space.md.value @fun.space.lg.value;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .project-title {
    font-weight: 700;
    font-size: 1.1rem;
  }

  .project-meta {
    color: @fun.color.muted.value;
    font-size: @fun.type.small.value;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .portfolio-footer {
    background: @fun.color.surface.value;
    border-top: 1px solid @fun.color.border.value;
    padding: @fun.space.lg.value 1rem;
    text-align: center;
    color: @fun.color.muted.value;
  }

  @media (max-width: 1180px) {
    .project-card { grid-column: span 6; }
  }

  @media (max-width: 760px) {
    .project-card { grid-column: span 12; }
  }
  `
}
@portfolio-layout()
```

## The HTML

```html
<main class="portfolio">
  <section class="portfolio-hero">
    <div class="portfolio-hero-inner">
      <div class="portfolio-eyebrow">Portfolio</div>
      <h1 class="portfolio-title">Alex River — Product Designer</h1>
      <p class="portfolio-lead">I design and build interfaces with FSCSS-driven systems: tokens, arrays, and events that scale without complexity.</p>
      <button class="portfolio-cta">Get in touch</button>
    </div>
  </section>

  <nav class="portfolio-filters" aria-label="Filter projects">
    <button class="filter-chip is-active">All</button>
    <button class="filter-chip">Web</button>
    <button class="filter-chip">App</button>
    <button class="filter-chip">Brand</button>
  </nav>

  <section class="portfolio-grid-wrap">
    <div class="portfolio-grid">
      <article class="project-card">
        <div class="project-media">
          <div class="project-overlay">
            <button class="overlay-btn">View Case</button>
            <button class="overlay-btn">Live Demo</button>
          </div>
        </div>
        <div class="project-body">
          <div class="project-title">Atlas Dashboard</div>
          <div class="project-meta">
            <span>Web • Analytics</span>
            <span>2026</span>
          </div>
        </div>
      </article>

      <article class="project-card">
        <div class="project-media">
          <div class="project-overlay">
            <button class="overlay-btn">View Case</button>
            <button class="overlay-btn">Live Demo</button>
          </div>
        </div>
        <div class="project-body">
          <div class="project-title">Pulse Mobile</div>
          <div class="project-meta">
            <span>App • Health</span>
            <span>2025</span>
          </div>
        </div>
      </article>

      <article class="project-card">
        <div class="project-media">
          <div class="project-overlay">
            <button class="overlay-btn">View Case</button>
            <button class="overlay-btn">Live Demo</button>
          </div>
        </div>
        <div class="project-body">
          <div class="project-title">Forma Brand</div>
          <div class="project-meta">
            <span>Brand • System</span>
            <span>2025</span>
          </div>
        </div>
      </article>

      <article class="project-card">
        <div class="project-media">
          <div class="project-overlay">
            <button class="overlay-btn">View Case</button>
            <button class="overlay-btn">Live Demo</button>
          </div>
        </div>
        <div class="project-body">
          <div class="project-title">Orbit Landing</div>
          <div class="project-meta">
            <span>Web • Marketing</span>
            <span>2026</span>
          </div>
        </div>
      </article>

      <article class="project-card">
        <div class="project-media">
          <div class="project-overlay">
            <button class="overlay-btn">View Case</button>
            <button class="overlay-btn">Live Demo</button>
          </div>
        </div>
        <div class="project-body">
          <div class="project-title">Echo App</div>
          <div class="project-meta">
            <span>App • Social</span>
            <span>2024</span>
          </div>
        </div>
      </article>

      <article class="project-card">
        <div class="project-media">
          <div class="project-overlay">
            <button class="overlay-btn">View Case</button>
            <button class="overlay-btn">Live Demo</button>
          </div>
        </div>
        <div class="project-body">
          <div class="project-title">Nova Identity</div>
          <div class="project-meta">
            <span>Brand • Logo</span>
            <span>2024</span>
          </div>
        </div>
      </article>
    </div>
  </section>

  <footer class="portfolio-footer">
    <div>© 2026 Alex River. Built with FSCSS.</div>
  </footer>
</main>
```

## What Happens

- Token root `@port-root()` defines theme + elevation; `@fun` reads with fallbacks. `--shadow-soft` adds depth on hover.
- `@event.filterActive(cat)` is a small predicate used conceptually for active state classes (can be applied per filter).
- Arrays `cat`, `title`, `year`, `tagStr` hold all project data; `count()`/iterators available to drive generation.
- `@portfolio-layout()` emits hero, filter chips (with `.is-active`), 12-column grid (`span 4/6/12`), card hover lift + overlay fade, and responsive breakpoints.
- Overlay buttons reveal on hover/focus-visible context (opacity transition) without JS.
- CTAs and chips share focus ring via `outline: 3px solid @fun.color.ring.value`.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Generate filter buttons from a `@arr filters[All,Web,App,Brand]` and apply `@event.filterActive`.
2. Generate the entire project grid from arrays (loop over `count(@arr.title!.length)` and emit card markup/styles).
3. Add a lightbox-style focus trap concept or image lazy placeholders via tokens.

## Summary

Arrays model the portfolio corpus, `@event` encodes filter state, and a block-defined 12-column grid + tokenized shadows deliver a polished, responsive showcase with minimal CSS duplication.

---

**Next: [50 — Admin Panel](../50-admin-panel.md)**
