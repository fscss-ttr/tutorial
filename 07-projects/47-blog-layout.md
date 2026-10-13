# 47 — Blog Layout

A content-first blog needs a readable main column, a metadata sidebar, and a responsive grid. This project demonstrates block-defined layouts, array-driven post metadata, and tokenized typography scales for long-form reading.

## The Brief

- Two-column blog layout: main post content and aside with author, tags, and recent posts.
- Drive tags and recent posts from arrays (no repeated markup in styles).
- Enforce a readable measure with max-width and typographic rhythm.
- Responsive: stack to one column under 960px.
- Use `@fun` tokens for type, spacing, surfaces, and links.

## The FSCSS

```fscss
/* Tokens */
@define blog-root(root: :root){
  `
  @use(root){
    --bg: #f8fafc;
    --surface: #ffffff;
    --ink: #0f172a;
    --muted: #475569;
    --border: rgba(15, 23, 42, 0.08);
    --accent: #2563eb;
    --accent-weak: rgba(37, 99, 235, 0.08);
    --radius: 14px;
    --shadow: 0 10px 30px rgba(15, 23, 42, 0.06);
  }
  `
}
@blog-root()

@fun(color){
  bg: var(--bg, #f8fafc);
  surface: var(--surface, #ffffff);
  ink: var(--ink, #0f172a);
  muted: var(--muted, #475569);
  border: var(--border, rgba(15,23,42,0.08));
  accent: var(--accent, #2563eb);
  accent-weak: var(--accent-weak, rgba(37,99,235,0.08));
}

@fun(space){
  xs: 0.75rem;
  sm: 1rem;
  md: 1.5rem;
  lg: 2rem;
  xl: 3rem;
}

@fun(type){
  body: 1.0625rem;
  lead: 1.25rem;
  h1: clamp(2rem, 4vw, 3.25rem);
  h2: clamp(1.5rem, 2.5vw, 2rem);
  h3: clamp(1.25rem, 2vw, 1.5rem);
  small: 0.95rem;
}

/* Data for aside */
@arr(tags[CSS, FSCSS, Preprocessor, Design Tokens, Layout])
@arr(recent[Arrays in Practice, Block Defines Demystified, Events Without JS, Loops That Scale])
@arr(i[count(@arr.tags!.length, 1)])
@arr(j[count(@arr.recent!.length, 1)])

/* Layout + content styles */
@define blog-layout(){
  `
  .blog {
    background: @fun.color.bg.value;
    color: @fun.color.ink.value;
    font-family: system-ui, -apple-system, sans-serif;
    line-height: 1.8;
    padding: @fun.space.xl.value 1rem;
  }

  .blog-inner {
    max-width: 1140px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: minmax(0, 2fr) minmax(280px, 1fr);
    gap: @fun.space.xl.value;
    align-items: start;
  }

  .blog-main {
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: 14px;
    box-shadow: 0 10px 30px rgba(15, 23, 42, 0.06);
    padding: clamp(1.5rem, 4vw, 3rem);
  }

  .blog-header {
    margin-bottom: @fun.space.lg.value;
  }

  .blog-eyebrow {
    color: @fun.color.accent.value;
    font-weight: 600;
    font-size: @fun.type.small.value;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  .blog-title {
    font-size: @fun.type.h1.value;
    line-height: 1.15;
    margin: 0.5rem 0 0.75rem;
  }

  .blog-lead {
    color: @fun.color.muted.value;
    font-size: @fun.type.lead.value;
    max-width: 70ch;
  }

  .blog-meta {
    color: @fun.color.muted.value;
    font-size: @fun.type.small.value;
    margin-top: @fun.space.sm.value;
    display: flex;
    gap: 0.75rem;
    flex-wrap: wrap;
  }

  .blog-prose {
    color: @fun.color.ink.value;
    font-size: @fun.type.body.value;
    max-width: 70ch;
  }

  .blog-prose p {
    margin: @fun.space.md.value 0;
  }

  .blog-prose h2 {
    font-size: @fun.type.h2.value;
    line-height: 1.2;
    margin: @fun.space.lg.value 0 @fun.space.sm.value;
  }

  .blog-prose h3 {
    font-size: @fun.type.h3.value;
    line-height: 1.25;
    margin: @fun.space.md.value 0 @fun.space.xs.value;
  }

  .blog-prose blockquote {
    margin: @fun.space.lg.value 0;
    padding-left: @fun.space.md.value;
    border-left: 4px solid @fun.color.accent.value;
    color: @fun.color.muted.value;
    font-style: italic;
  }

  .blog-prose pre {
    background: #0f172a;
    color: #e2e8f0;
    border-radius: 12px;
    padding: @fun.space.md.value;
    overflow-x: auto;
    font-size: 1rem;
    line-height: 1.7;
  }

  .blog-aside {
    display: flex;
    flex-direction: column;
    gap: @fun.space.md.value;
  }

  .aside-card {
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: 14px;
    box-shadow: 0 10px 30px rgba(15, 23, 42, 0.06);
    padding: @fun.space.md.value @fun.space.lg.value;
  }

  .aside-title {
    font-size: 1rem;
    font-weight: 700;
    margin-bottom: @fun.space.sm.value;
  }

  .aside-text {
    color: @fun.color.muted.value;
    font-size: @fun.type.small.value;
    line-height: 1.7;
  }

  .tag-list {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .tag-link {
    display: inline-flex;
    align-items: center;
    padding: 0.375rem 0.75rem;
    border-radius: 999px;
    background: @fun.color.accent-weak.value;
    color: @fun.color.accent.value;
    text-decoration: none;
    font-size: @fun.type.small.value;
    font-weight: 600;
  }

  .tag-link:hover {
    filter: brightness(0.98);
  }

  .recent-list {
    display: flex;
    flex-direction: column;
    gap: 0.625rem;
  }

  .recent-link {
    color: @fun.color.ink.value;
    text-decoration: none;
    font-weight: 600;
    font-size: @fun.type.small.value;
    line-height: 1.6;
  }

  .recent-link:hover {
    color: @fun.color.accent.value;
  }

  @media (max-width: 960px) {
    .blog-inner {
      grid-template-columns: 1fr;
    }
    .blog-aside {
      order: -1;
    }
  }
  `
}
@blog-layout()
```

## The HTML

```html
<article class="blog">
  <div class="blog-inner">
    <main class="blog-main">
      <header class="blog-header">
        <div class="blog-eyebrow">Advanced Series</div>
        <h1 class="blog-title">Building Checkout Flows with FSCSS</h1>
        <p class="blog-lead">Arrays, events, and block defines let you model real UI logic without pulling a JS framework into your styles.</p>
        <div class="blog-meta">
          <span>By Alex River</span>
          <span>•</span>
          <span>Published Oct 13, 2026</span>
          <span>•</span>
          <span>8 min read</span>
        </div>
      </header>

      <div class="blog-prose">
        <p>Most preprocessors stop at nesting and variables. FSCSS goes further: arrays drive repetition, events encode decisions, and block defines emit whole rule sets. Together they collapse boilerplate.</p>

        <h2>Model your data</h2>
        <p>Keep pricing, labels, and routes in <code>@arr</code>. Walk them with <code>count()</code> and <code>@arr.i[]</code> to generate CSS or to accumulate values with <code>num()</code>.</p>

        <h2>Encode business rules</h2>
        <p><code>@event taxRate(region)</code> turns thresholds into testable logic. Return values inline where they're needed: <code>$taxCents: num($subtotalCents! * @event.taxRate("usd"));</code>.</p>

        <h2>Emit full layouts</h2>
        <p>Block defines wrapped in backticks let you emit complete blocks — including media queries — from one call. Scope with selector params when reusing.</p>

        <blockquote>“If you find yourself copy-pasting three classes, put it in an array or a define.”</blockquote>

        <p>That rule scales from buttons to entire pages without losing readability.</p>

        <h3>Tips</h3>
        <p>Prefer token roots for shared custom properties, <code>@fun</code> for semantic access, and read-only computed values for derived totals.</p>
      </div>
    </main>

    <aside class="blog-aside">
      <section class="aside-card">
        <h2 class="aside-title">Author</h2>
        <p class="aside-text">Alex River is a frontend engineer exploring build-time CSS. They write about preprocessors, design systems, and ergonomic tooling.</p>
      </section>

      <section class="aside-card">
        <h2 class="aside-title">Tags</h2>
        <nav class="tag-list" aria-label="Post tags">
          <a class="tag-link" href="#">CSS</a>
          <a class="tag-link" href="#">FSCSS</a>
          <a class="tag-link" href="#">Preprocessor</a>
          <a class="tag-link" href="#">Design Tokens</a>
          <a class="tag-link" href="#">Layout</a>
        </nav>
      </section>

      <section class="aside-card">
        <h2 class="aside-title">Recent Posts</h2>
        <nav class="recent-list" aria-label="Recent posts">
          <a class="recent-link" href="#">Arrays in Practice</a>
          <a class="recent-link" href="#">Block Defines Demystified</a>
          <a class="recent-link" href="#">Events Without JS</a>
          <a class="recent-link" href="#">Loops That Scale</a>
        </nav>
      </section>
    </aside>
  </div>
</article>
```

## What Happens

- `@blog-root()` emits a single token source (`:root`) for cross-page reuse; `@fun` provides semantic aliases with fallbacks.
- Arrays `tags` and `recent` are defined and indexed via `count()`/iterators for potential generation; the HTML shown is illustrative while styles remain data-driven.
- `@blog-layout()` emits the grid, main column, prose rhythm (`max-width: 70ch`), aside cards, tags, recent links, and a `max-width: 960px` stack.
- Type scale uses `clamp()` via `@fun.type` to keep headings fluid without breakpoint creep.
- Aside moves to top on small screens (`order: -1`) to preserve content hierarchy in stacked layout.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Generate tag links from `@arr.tags` and `@arr.i[]` into the aside (styling-only or structure via block string).
2. Style code blocks with a tokenized syntax palette and inline numbers.
3. Add a reading progress indicator tied to scroll (conceptual) or a sticky table of contents.

## Summary

Token roots + array data + block-defined layout deliver a readable, responsive blog. Typographic tokens enforce measure and rhythm, while the aside stays predictable and skinnable.

---

**Next: [48 — Docs Layout](../48-docs-layout.md)**
