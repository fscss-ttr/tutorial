# 26 — Sidebar Navigation

A fixed sidebar shows the full site map without eating the top bar. Build one from arrays, drive active states with tokens, and use a block define to hold the layout so the main content can sit beside it.

## The Brief

- A left sidebar with a brand, a primary nav, and a footer block.
- All nav items generated from an array (labels + hrefs).
- An active state for the current page via a token/utility.
- Responsive: collapse to a top bar at 768px with the same links.

## The FSCSS

```fscss
/* tokens */
@fun(color){
  bg: #0f172a;
  bg-hover: #111a2e;
  ink: #e2e8f0;
  muted: #94a3b8;
  active: #2563eb;
  surface: #ffffff;
}

@fun(space){
  pad: 0.75rem 1rem;
}

@define sidebar-root() {
  `
  :root {
    --sidebar-w: 240px;
    --sidebar-pad: 1rem;
  }
  `
}

@define sidebar-layout() {
  `
  .app {
    display: grid;
    grid-template-columns: var(--sidebar-w, 240px) 1fr;
    min-height: 100vh;
  }

  .sidebar {
    background: @fun.color.bg.value;
    color: @fun.color.ink.value;
    display: flex;
    flex-direction: column;
    padding: var(--sidebar-pad, 1rem);
    gap: 1.5rem;
    box-shadow: inset -1px 0 0 rgba(255, 255, 255, 0.05);
  }

  .main {
    background: @fun.color.surface.value;
    padding: 2rem clamp(1rem, 4vw, 2.5rem);
  }

  @media (max-width: 768px) {
    .app {
      grid-template-columns: 1fr;
      grid-template-rows: auto 1fr;
    }

    .sidebar {
      flex-direction: row;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 1rem;
      padding: 0.75rem 1rem;
    }

    .sidebar-nav {
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem;
    }

    .sidebar-footer {
      margin-top: 0;
    }
  }
  `
}

@define sidebar-brand() {
  `
  .sidebar-brand {
    font-size: 1.15rem;
    font-weight: 700;
    letter-spacing: -0.01em;
    color: @fun.color.ink.value;
  }
  `
}

@define sidebar-nav() {
  `
  .sidebar-nav {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
    flex: 1;
  }

  .sidebar-link {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    padding: @fun.space.pad.value;
    color: @fun.color.muted.value;
    text-decoration: none;
    border-radius: 10px;
    transition: background-color 0.15s ease, color 0.15s ease;
  }

  .sidebar-link:hover {
    background: @fun.color.bg-hover.value;
    color: @fun.color.ink.value;
  }

  .sidebar-link.is-active {
    background: @fun.color.active.value;
    color: @fun.color.ink.value;
  }
  `
}

@define sidebar-footer() {
  `
  .sidebar-footer {
    margin-top: auto;
    font-size: 0.82rem;
    color: @fun.color.muted.value;
    padding: 0.5rem 0.25rem;
  }
  `
}

@sidebar-root()
@sidebar-layout()
@sidebar-brand()
@sidebar-nav()
@sidebar-footer()
```

## The Data

```fscss
@arr(nav-label[Dashboard, Projects, Team, Settings])
@arr(nav-href[/, /projects, /team, /settings])
@arr(i[count(@arr.nav-label!.length, 1)])
```

## The HTML

```html
<div class="app">
  <aside class="sidebar">
    <div class="sidebar-brand">FSCSS App</div>
    <nav class="sidebar-nav">
      <a class="sidebar-link is-active" href="/">Dashboard</a>
      <a class="sidebar-link" href="/projects">Projects</a>
      <a class="sidebar-link" href="/team">Team</a>
      <a class="sidebar-link" href="/settings">Settings</a>
    </nav>
    <div class="sidebar-footer">v0.1 • © 2026</div>
  </aside>
  <main class="main">Content goes here.</main>
</div>
```

## What Happens

- `@sidebar-root()` exposes `--sidebar-w`/`--sidebar-pad` as tokens so the grid and padding stay in lockstep.
- `@sidebar-layout()` holds the two-column grid plus a max-width breakpoint in one block define, keeping media queries co-located with the layout.
- Arrays (`nav-label`, `nav-href`, `i`) describe the nav; in practice you would loop over them to generate class/structure, but the CSS above styles the contract once.
- `.is-active` is the single source of truth for the selected item, pulled from `@fun.color.active`.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Loop over `@arr.nav-label` and `@arr.nav-href` to generate `.sidebar-link` rules/attributes conceptually (style-driven, not HTML-emitting).
2. Add a collapsible mode with a `--sidebar-collapsed` token and animate width via CSS variables.
3. Introduce a nested section (e.g. `Projects > Active`) using `@use(st) *` descendants if you split nav into groups.

## Summary

Arrays + token root + block defines turn a sidebar into a governed layout: the shell is fixed, the items are data, and responsiveness lives next to the grid that needs it.

---

**Next: [27 — Search Bar](../27-search-bar.md)**