# 48 — Docs Layout

Documentation needs a left nav, a main content area with anchors, and often a right TOC. This project shows how to model nested navigation with arrays, emit a full docs shell via a block define, and keep headings/anchors accessible.

## The Brief

- Three-column docs layout: sidebar (docs nav), main (content), and TOC (on-page links).
- Drive sidebar sections and TOC entries from arrays.
- Sticky sidebar/TOC with independent scroll.
- Responsive: collapse TOC under 1200px, stack nav+content under 980px.
- Tokenized type, surfaces, and active states.

## The FSCSS

```fscss
/* Tokens */
@define docs-root(root: :root){
  `
  @use(root){
    --bg: #f8fafc;
    --surface: #ffffff;
    --ink: #0f172a;
    --muted: #475569;
    --border: rgba(15, 23, 42, 0.08);
    --accent: #2563eb;
    --accent-weak: rgba(37, 99, 235, 0.08);
    --code-bg: #0f172a;
    --code-ink: #e2e8f0;
    --radius: 12px;
    --shadow: 0 10px 30px rgba(15, 23, 42, 0.05);
  }
  `
}
@docs-root()

@fun(color){
  bg: var(--bg, #f8fafc);
  surface: var(--surface, #ffffff);
  ink: var(--ink, #0f172a);
  muted: var(--muted, #475569);
  border: var(--border, rgba(15,23,42,0.08));
  accent: var(--accent, #2563eb);
  accent-weak: var(--accent-weak, rgba(37,99,235,0.08));
  code-bg: var(--code-bg, #0f172a);
  code-ink: var(--code-ink, #e2e8f0);
}

@fun(space){
  xs: 0.5rem;
  sm: 0.75rem;
  md: 1rem;
  lg: 1.5rem;
  xl: 2rem;
}

@fun(type){
  body: 1.02rem;
  h1: clamp(2rem, 4vw, 3rem);
  h2: clamp(1.5rem, 2.5vw, 1.9rem);
  h3: clamp(1.25rem, 2vw, 1.4rem);
  small: 0.95rem;
}

@fun(radius){
  md: var(--radius, 12px);
}

@fun(shadow){
  md: var(--shadow, 0 10px 30px rgba(15, 23, 42, 0.05));
}

/* Nav data */
@arr(section[Getting Started, Concepts, API, Guides])
@arr(getting[Installation, Quickstart, CLI])
@arr(concepts[Tokens, Defines, Arrays, Events])
@arr(api[@define, @fun, @arr, @event, @import])
@arr(guides[Checkout, Blog, Docs, Portfolio, Admin])

/* TOC data */
@arr(toc[Overview, Installation, Arrays, Events, Block Defines])

/* Layout */
@define docs-layout(){
  `
  .docs {
    background: @fun.color.bg.value;
    color: @fun.color.ink.value;
    font-family: system-ui, -apple-system, sans-serif;
    line-height: 1.8;
  }

  .docs-header {
    background: @fun.color.surface.value;
    border-bottom: 1px solid @fun.color.border.value;
    padding: 0.875rem @fun.space.xl.value;
    position: sticky;
    top: 0;
    z-index: 5;
    box-shadow: 0 1px 0 rgba(15, 23, 42, 0.02);
  }

  .docs-header-inner {
    max-width: 1320px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: @fun.space.md.value;
  }

  .docs-brand {
    font-weight: 700;
    letter-spacing: -0.01em;
  }

  .docs-search {
    border: 1px solid @fun.color.border.value;
    border-radius: 999px;
    padding: 0.5rem 0.875rem;
    font: inherit;
    min-width: 260px;
    background: @fun.color.surface.value;
  }

  .docs-shell {
    max-width: 1320px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: 280px minmax(0, 1fr) 240px;
    gap: @fun.space.xl.value;
    padding: @fun.space.xl.value 1rem;
    align-items: start;
  }

  .docs-nav {
    position: sticky;
    top: 76px;
    max-height: calc(100vh - 96px);
    overflow-y: auto;
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: @fun.radius.md.value;
    padding: @fun.space.md.value @fun.space.lg.value;
    box-shadow: @fun.shadow.md.value;
  }

  .nav-section + .nav-section {
    margin-top: @fun.space.lg.value;
  }

  .nav-title {
    font-weight: 700;
    font-size: @fun.type.small.value;
    margin-bottom: @fun.space.xs.value;
  }

  .nav-list {
    display: flex;
    flex-direction: column;
    gap: 0.375rem;
  }

  .nav-link {
    color: @fun.color.muted.value;
    text-decoration: none;
    font-size: @fun.type.small.value;
    padding: 0.375rem 0.5rem;
    border-radius: 8px;
  }

  .nav-link:hover {
    color: @fun.color.ink.value;
    background: @fun.color.accent-weak.value;
  }

  .nav-link--active {
    color: @fun.color.accent.value;
    background: @fun.color.accent-weak.value;
    font-weight: 600;
  }

  .docs-main {
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: @fun.radius.md.value;
    box-shadow: @fun.shadow.md.value;
    padding: clamp(1.5rem, 4vw, 3rem);
  }

  .docs-prose {
    max-width: 78ch;
    color: @fun.color.ink.value;
    font-size: @fun.type.body.value;
  }

  .docs-prose h1 {
    font-size: @fun.type.h1.value;
    line-height: 1.15;
    margin: 0 0 @fun.space.md.value;
  }

  .docs-prose h2 {
    font-size: @fun.type.h2.value;
    line-height: 1.2;
    margin: @fun.space.lg.value 0 @fun.space.sm.value;
  }

  .docs-prose h3 {
    font-size: @fun.type.h3.value;
    line-height: 1.25;
    margin: @fun.space.md.value 0 @fun.space.xs.value;
  }

  .docs-prose p {
    margin: @fun.space.md.value 0;
  }

  .docs-prose code {
    background: @fun.color.code-bg.value;
    color: @fun.color.code-ink.value;
    border-radius: 6px;
    padding: 0.15rem 0.4rem;
    font-size: 0.98rem;
  }

  .docs-prose pre {
    background: @fun.color.code-bg.value;
    color: @fun.color.code-ink.value;
    border-radius: 12px;
    padding: @fun.space.md.value;
    overflow-x: auto;
    margin: @fun.space.lg.value 0;
    line-height: 1.8;
  }

  .docs-toc {
    position: sticky;
    top: 76px;
    max-height: calc(100vh - 96px);
    overflow-y: auto;
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: @fun.radius.md.value;
    padding: @fun.space.md.value @fun.space.lg.value;
    box-shadow: @fun.shadow.md.value;
  }

  .toc-title {
    font-weight: 700;
    font-size: @fun.type.small.value;
    margin-bottom: @fun.space.sm.value;
  }

  .toc-list {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .toc-link {
    color: @fun.color.muted.value;
    text-decoration: none;
    font-size: @fun.type.small.value;
    line-height: 1.6;
  }

  .toc-link:hover { color: @fun.color.ink.value; }
  .toc-link--sub { padding-left: @fun.space.sm.value; }

  @media (max-width: 1200px) {
    .docs-shell {
      grid-template-columns: 280px minmax(0, 1fr);
    }
    .docs-toc {
      display: none;
    }
  }

  @media (max-width: 980px) {
    .docs-shell {
      grid-template-columns: 1fr;
    }
    .docs-nav {
      position: static;
      max-height: none;
      order: -1;
    }
    .docs-header-inner {
      flex-direction: column;
      align-items: flex-start;
    }
  }
  `
}
@docs-layout()
```

## The HTML

```html
<header class="docs-header">
  <div class="docs-header-inner">
    <div class="docs-brand">FSCSS Docs</div>
    <input class="docs-search" type="search" placeholder="Search docs…" aria-label="Search docs">
  </div>
</header>

<div class="docs">
  <div class="docs-shell">
    <nav class="docs-nav" aria-label="Docs navigation">
      <div class="nav-section">
        <div class="nav-title">Getting Started</div>
        <div class="nav-list">
          <a class="nav-link" href="#">Installation</a>
          <a class="nav-link nav-link--active" href="#">Quickstart</a>
          <a class="nav-link" href="#">CLI</a>
        </div>
      </div>
      <div class="nav-section">
        <div class="nav-title">Concepts</div>
        <div class="nav-list">
          <a class="nav-link" href="#">Tokens</a>
          <a class="nav-link" href="#">Defines</a>
          <a class="nav-link" href="#">Arrays</a>
          <a class="nav-link" href="#">Events</a>
        </div>
      </div>
      <div class="nav-section">
        <div class="nav-title">API</div>
        <div class="nav-list">
          <a class="nav-link" href="#">@define</a>
          <a class="nav-link" href="#">@fun</a>
          <a class="nav-link" href="#">@arr</a>
          <a class="nav-link" href="#">@event</a>
          <a class="nav-link" href="#">@import</a>
        </div>
      </div>
      <div class="nav-section">
        <div class="nav-title">Guides</div>
        <div class="nav-list">
          <a class="nav-link" href="#">Checkout</a>
          <a class="nav-link" href="#">Blog</a>
          <a class="nav-link" href="#">Docs</a>
          <a class="nav-link" href="#">Portfolio</a>
          <a class="nav-link" href="#">Admin</a>
        </div>
      </div>
    </nav>

    <main class="docs-main">
      <article class="docs-prose">
        <h1>Quickstart</h1>
        <p>Install FSCSS and compile your first file. This guide walks you through CLI usage and the CDN runtime for quick prototyping.</p>

        <h2 id="installation">Installation</h2>
        <p>Use your preferred package manager or download the binary. Verify with <code>fscss --version</code>.</p>

        <h2 id="arrays">Arrays</h2>
        <p><code>@arr</code> holds lists of values. Walk with <code>count()</code> and <code>@arr.i[]</code> to generate repeated CSS without duplication.</p>

        <h2 id="events">Events</h2>
        <p><code>@event</code> is a small build-time switch: test conditions and <code>return</code> values used directly in declarations.</p>

        <h2 id="block-defines">Block Defines</h2>
        <p>Wrap a body in backticks inside <code>@define</code> to emit entire rule sets. Accept a selector param to scope descendants.</p>

        <pre><code>@define card(st: .card){
  `
  @use(st){
    background: white;
    border-radius: 12px;
    padding: 1.5rem;
  }
  `
}
@card()</code></pre>

        <p>Call once, reuse everywhere.</p>
      </article>
    </main>

    <aside class="docs-toc" aria-label="On this page">
      <div class="toc-title">On this page</div>
      <nav class="toc-list">
        <a class="toc-link" href="#installation">Installation</a>
        <a class="toc-link toc-link--sub" href="#arrays">Arrays</a>
        <a class="toc-link toc-link--sub" href="#events">Events</a>
        <a class="toc-link" href="#block-defines">Block Defines</a>
      </nav>
    </aside>
  </div>
</div>
```

## What Happens

- Token root `@docs-root()` centralizes theme variables (bg, surface, code blocks). `@fun` exposes semantic names.
- Arrays `section`, `getting`, `concepts`, `api`, `guides`, and `toc` model nav structure for generation/iteration.
- `@docs-layout()` emits the header, 3-column shell, sticky nav/TOC, active link state, prose with `max-width: 78ch`, code/pre styles, and responsive breakpoints.
- Sticky positioning uses `top: 76px` with `max-height: calc(100vh - 96px)` and `overflow-y: auto` for independent scroll.
- Breakpoints: `<1200px` hides TOC; `<980px` stacks into single column and makes nav static with `order: -1`.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Generate the entire sidebar from arrays (recursive or flat) instead of hardcoded sections.
2. Build an auto-TOC from `.docs-prose h2/h3` conceptually, or style generated anchors.
3. Add a light/dark theme toggle via `@event` and token root override.

## Summary

Arrays map the doc tree, a block define emits the sticky shell, and tokenized code/prose keep long-form docs readable and navigable across breakpoints.

---

**Next: [49 — Portfolio Page](../49-portfolio-page.md)**
