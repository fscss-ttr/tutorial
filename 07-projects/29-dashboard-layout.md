# 29 — Dashboard Layout

Dashboards are the most layout-heavy page in the set. Stitch header, sidebar, content, and a stat strip into one app shell using token root + block defines + arrays for the stat cards.

## The Brief

- A classic `.app` grid: `.app-header`, `.app-sidebar`, `.app-main`.
- A `.stats` strip with four stat cards, driven by arrays.
- Content area with a primary card and a two-column subgrid.
- Responsive: stack sidebar under the header and collapse columns below 900px.

## The FSCSS

```fscss
@fun(color){
  bg: #f8fafc;
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  border: rgba(15, 23, 42, 0.06);
  accent: #2563eb;
}

@fun(space){
  pad: 1.25rem 1.5rem;
  gap: 1rem;
}

@define dashboard-root() {
  `
  :root {
    --sidebar-w: 260px;
    --header-h: 64px;
    --radius: 12px;
  }
  `
}

@define dashboard-shell() {
  `
  .app {
    display: grid;
    grid-template-columns: var(--sidebar-w, 260px) 1fr;
    grid-template-rows: var(--header-h, 64px) 1fr;
    grid-template-areas:
      "sidebar header"
      "sidebar main";
    min-height: 100vh;
    background: @fun.color.bg.value;
  }

  .app-header {
    grid-area: header;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 1.5rem;
    background: @fun.color.surface.value;
    border-bottom: 1px solid @fun.color.border.value;
  }

  .app-sidebar {
    grid-area: sidebar;
    background: #0f172a;
    color: #e2e8f0;
    padding: 1.25rem 1rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
    box-shadow: inset -1px 0 0 rgba(255, 255, 255, 0.05);
  }

  .app-main {
    grid-area: main;
    padding: clamp(1rem, 3vw, 2rem);
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }

  @media (max-width: 1080px) {
    .app {
      grid-template-columns: 1fr;
      grid-template-rows: auto auto 1fr;
      grid-template-areas:
        "header"
        "sidebar"
        "main";
    }
  }
  `
}

@define dashboard-stats() {
  `
  .stats {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: @fun.space.gap.value;
  }

  .stat-card {
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: var(--radius, 12px);
    padding: @fun.space.pad.value;
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
    box-shadow: 0 6px 24px rgba(15, 23, 42, 0.04);
  }

  .stat-label {
    color: @fun.color.muted.value;
    font-size: 0.9rem;
  }

  .stat-value {
    color: @fun.color.ink.value;
    font-size: clamp(1.6rem, 4vw, 2.6rem);
    font-weight: 700;
    line-height: 1.1;
  }

  .stat-delta {
    color: @fun.color.accent.value;
    font-size: 0.9rem;
  }

  @media (max-width: 1080px) {
    .stats { grid-template-columns: repeat(2, minmax(0, 1fr)); }
  }

  @media (max-width: 640px) {
    .stats { grid-template-columns: 1fr; }
  }
  `
}

@define dashboard-panels() {
  `
  .panel-grid {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: @fun.space.gap.value;
  }

  .panel {
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: var(--radius, 12px);
    padding: @fun.space.pad.value;
    box-shadow: 0 8px 30px rgba(15, 23, 42, 0.05);
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
    min-height: 280px;
  }

  .panel-title {
    color: @fun.color.ink.value;
    font-weight: 700;
    font-size: 1.05rem;
  }

  .panel-body {
    color: @fun.color.muted.value;
    line-height: 1.7;
    flex: 1;
  }

  @media (max-width: 900px) {
    .panel-grid { grid-template-columns: 1fr; }
  }
  `
}

@dashboard-root()
@dashboard-shell()
@dashboard-stats()
@dashboard-panels()
```

## The Data

```fscss
@arr(stat-label[Users, Revenue, Orders, Churn])
@arr(stat-value[12.4k, $84.2k, 3,912, 2.8%])
@arr(stat-delta[+8.1%, +12.4%, +6.2%, -0.6%])
@arr(i[count(@arr.stat-label!.length, 1)])
```

## The HTML

```html
<div class="app">
  <header class="app-header">
    <div class="app-brand">Dashboard</div>
    <div class="app-actions">Search • Profile</div>
  </header>
  <aside class="app-sidebar">
    <div>Overview</div>
    <div>Reports</div>
    <div>Customers</div>
    <div>Billing</div>
  </aside>
  <main class="app-main">
    <section class="stats">
      <article class="stat-card">
        <div class="stat-label">Users</div>
        <div class="stat-value">12.4k</div>
        <div class="stat-delta">+8.1%</div>
      </article>
      <article class="stat-card">
        <div class="stat-label">Revenue</div>
        <div class="stat-value">$84.2k</div>
        <div class="stat-delta">+12.4%</div>
      </article>
      <article class="stat-card">
        <div class="stat-label">Orders</div>
        <div class="stat-value">3,912</div>
        <div class="stat-delta">+6.2%</div>
      </article>
      <article class="stat-card">
        <div class="stat-label">Churn</div>
        <div class="stat-value">2.8%</div>
        <div class="stat-delta">-0.6%</div>
      </article>
    </section>
    <section class="panel-grid">
      <article class="panel">
        <div class="panel-title">Activity</div>
        <div class="panel-body">Recent events, commits, and alerts.</div>
      </article>
      <article class="panel">
        <div class="panel-title">Tasks</div>
        <div class="panel-body">P0–P3 queue with owners and due dates.</div>
      </article>
    </section>
  </main>
</div>
```

## What Happens

- `dashboard-root()` defines `--sidebar-w`, `--header-h`, `--radius` so header height, sidebar width, and card radii never drift.
- `grid-template-areas` expresses the canonical shell; the 1080px breakpoint reflows to a vertical stack while keeping sidebar semantics intact.
- Stats are data-driven (`stat-label/value/delta/i` + `count`) — one card block styles all four; panels reuse the same radius/border tokens.
- `clamp()` on `.stat-value` and `.app-main` padding keeps type and gutters fluid without extra breakpoints.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Generate the `.stat-card` set from the arrays (conceptually) and vary a subtle tint via `@random` per card.
2. Add a `.app-nav` inside the sidebar generated from an array of links (labels + hrefs), with an `.is-active` state.
3. Turn the header into a `str()` block or extract it to a reusable define shared with 26/27.

## Summary

A real dashboard shell: named grid areas, tokenized geometry, array-driven stats, and co-located breakpoints. All stitched from block defines with zero one-off overrides.

---

**Next: [30 — Login Page](../30-login-page.md)**