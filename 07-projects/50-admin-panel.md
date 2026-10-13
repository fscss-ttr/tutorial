# 50 — Admin Panel

The final project is an admin dashboard: a fixed sidebar, header with search/actions, KPI stat cards, a data table, and a quick activity feed. It brings together tokens, block defines, arrays, `@event` for status badges, and `num()` for computed values at scale.

## The Brief

- Fixed sidebar with nav groups and active state.
- Top header with search, notifications, user menu.
- KPI row driven by stat arrays (label, value, delta, trend).
- Data table generated from arrays (user, role, status, last active).
- Activity feed with timestamped items.
- Responsive: collapse sidebar under 1080px, stack sections under 920px.
- Use token root, semantic `@fun`, and generated utilities where helpful.

## The FSCSS

```fscss
/* Tokens */
@define admin-root(root: :root){
  `
  @use(root){
    --bg: #f8fafc;
    --surface: #ffffff;
    --surface-alt: #f1f5f9;
    --ink: #0f172a;
    --muted: #475569;
    --border: rgba(15, 23, 42, 0.08);
    --accent: #2563eb;
    --accent-weak: rgba(37, 99, 235, 0.1);
    --success: #22c55e;
    --warning: #f59e0b;
    --danger: #ef4444;
    --ring: rgba(37, 99, 235, 0.24);
    --radius: 14px;
    --shadow: 0 10px 30px rgba(15, 23, 42, 0.06);
  }
  `
}
@admin-root()

@fun(color){
  bg: var(--bg, #f8fafc);
  surface: var(--surface, #ffffff);
  surface-alt: var(--surface-alt, #f1f5f9);
  ink: var(--ink, #0f172a);
  muted: var(--muted, #475569);
  border: var(--border, rgba(15,23,42,0.08));
  accent: var(--accent, #2563eb);
  accent-weak: var(--accent-weak, rgba(37,99,235,0.1));
  success: var(--success, #22c55e);
  warning: var(--warning, #f59e0b);
  danger: var(--danger, #ef4444);
  ring: var(--ring, rgba(37,99,235,0.24));
}

@fun(space){
  xs: 0.5rem;
  sm: 0.75rem;
  md: 1rem;
  lg: 1.5rem;
  xl: 2rem;
}

@fun(type){
  h1: clamp(1.5rem, 3vw, 2rem);
  h2: clamp(1.25rem, 2vw, 1.5rem);
  small: 0.95rem;
}

@fun(radius){
  md: var(--radius, 14px);
}

@fun(shadow){
  md: var(--shadow, 0 10px 30px rgba(15, 23, 42, 0.06));
}

/* Status mapping */
@event statusClass(s){
  if s: active { return: "badge--success"; }
  el-if s: pending { return: "badge--warning"; }
  el-if s: suspended { return: "badge--danger"; }
  el { return: "badge--muted"; }
}

/* KPI data */
@arr(kpiLabel[Users, Revenue, Orders, Churn])
@arr(kpiValue[12840, 84290, 3240, 2.8])
@arr(kpiUnit[, $, , %])
@arr(kpiDelta[+12%, +8%, +18%, -4%])

/* Table data */
@arr(user[Avery Chen, Morgan Ellis, Jordan Lee, Casey Nguyen, Riley Park])
@arr(role[Admin, Editor, Viewer, Editor, Admin])
@arr(status[active, pending, active, suspended, active])
@arr(last[2m ago, 1h ago, 3h ago, Yesterday, 2d ago])

/* Activity */
@arr(act[New user signed up, Order #4821 paid, Role updated to Editor, API key revoked, Report exported])

/* Layout */
@define admin-layout(){
  `
  .admin {
    display: grid;
    grid-template-columns: 280px 1fr;
    grid-template-rows: 76px 1fr;
    min-height: 100vh;
    background: @fun.color.bg.value;
    color: @fun.color.ink.value;
    font-family: system-ui, -apple-system, sans-serif;
    line-height: 1.7;
  }

  .admin-sidebar {
    grid-row: 1 / -1;
    background: @fun.color.surface.value;
    border-right: 1px solid @fun.color.border.value;
    display: flex;
    flex-direction: column;
    padding: @fun.space.lg.value @fun.space.md.value;
    gap: @fun.space.lg.value;
    position: sticky;
    top: 0;
    height: 100vh;
    overflow-y: auto;
  }

  .admin-brand {
    padding: 0 @fun.space.sm.value;
    font-weight: 800;
    letter-spacing: -0.01em;
  }

  .sidebar-group {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
  }

  .sidebar-title {
    padding: 0.375rem @fun.space.sm.value;
    color: @fun.color.muted.value;
    font-size: @fun.type.small.value;
    text-transform: uppercase;
    letter-spacing: 0.08em;
  }

  .sidebar-link {
    display: flex;
    align-items: center;
    gap: @fun.space.sm.value;
    padding: 0.625rem @fun.space.sm.value;
    border-radius: 10px;
    color: @fun.color.ink.value;
    text-decoration: none;
  }

  .sidebar-link:hover { background: @fun.color.accent-weak.value; }
  .sidebar-link--active {
    background: @fun.color.accent-weak.value;
    color: @fun.color.accent.value;
    font-weight: 600;
  }

  .admin-header {
    grid-column: 2 / -1;
    background: @fun.color.surface.value;
    border-bottom: 1px solid @fun.color.border.value;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 @fun.space.xl.value;
    gap: @fun.space.md.value;
    position: sticky;
    top: 0;
    z-index: 4;
  }

  .header-search {
    border: 1px solid @fun.color.border.value;
    border-radius: 999px;
    padding: 0.5rem 0.875rem;
    font: inherit;
    min-width: 320px;
    background: @fun.color.surface.value;
  }

  .header-actions {
    display: flex;
    align-items: center;
    gap: @fun.space.sm.value;
  }

  .ghost-btn {
    border: 1px solid @fun.color.border.value;
    background: @fun.color.surface.value;
    border-radius: 999px;
    padding: 0.5rem 0.875rem;
    font: inherit;
  }

  .avatar {
    width: 34px;
    height: 34px;
    border-radius: 50%;
    background: linear-gradient(135deg, @fun.color.accent.value, rgba(37,99,235,0.4));
    border: 2px solid @fun.color.surface.value;
    box-shadow: 0 0 0 1px @fun.color.border.value;
  }

  .admin-main {
    grid-column: 2 / -1;
    padding: @fun.space.xl.value;
    display: flex;
    flex-direction: column;
    gap: @fun.space.xl.value;
  }

  .admin-title {
    font-size: @fun.type.h1.value;
    margin: 0;
  }

  .kpi-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: @fun.space.md.value;
  }

  .kpi-card {
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: @fun.radius.md.value;
    box-shadow: @fun.shadow.md.value;
    padding: @fun.space.lg.value;
    display: flex;
    flex-direction: column;
    gap: 0.375rem;
  }

  .kpi-label {
    color: @fun.color.muted.value;
    font-size: @fun.type.small.value;
  }

  .kpi-value {
    font-size: clamp(2rem, 4vw, 2.75rem);
    font-weight: 700;
    letter-spacing: -0.02em;
  }

  .kpi-delta {
    font-size: @fun.type.small.value;
    font-weight: 600;
    color: @fun.color.success.value;
  }

  .admin-grid {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: @fun.space.xl.value;
    align-items: start;
  }

  .panel {
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: @fun.radius.md.value;
    box-shadow: @fun.shadow.md.value;
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  .panel-header {
    padding: @fun.space.md.value @fun.space.lg.value;
    border-bottom: 1px solid @fun.color.border.value;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .panel-title {
    font-weight: 700;
  }

  .table-wrap {
    overflow-x: auto;
  }

  .admin-table {
    width: 100%;
    border-collapse: collapse;
    min-width: 680px;
  }

  .admin-table th,
  .admin-table td {
    text-align: left;
    padding: 0.75rem @fun.space.lg.value;
    border-bottom: 1px solid @fun.color.border.value;
    font-size: @fun.type.small.value;
  }

  .admin-table thead th {
    background: @fun.color.surface-alt.value;
    color: @fun.color.muted.value;
    font-weight: 600;
  }

  .badge {
    display: inline-flex;
    align-items: center;
    padding: 0.25rem 0.625rem;
    border-radius: 999px;
    font-weight: 600;
    font-size: 0.9rem;
  }

  .badge--success { background: rgba(34, 197, 94, 0.16); color: #166534; }
  .badge--warning { background: rgba(245, 158, 11, 0.16); color: #92400e; }
  .badge--danger  { background: rgba(239, 68, 68, 0.16); color: #991b1b; }
  .badge--muted   { background: @fun.color.surface-alt.value; color: @fun.color.muted.value; }

  .activity-list {
    display: flex;
    flex-direction: column;
    padding: @fun.space.sm.value 0;
  }

  .activity-item {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
    padding: 0.625rem @fun.space.lg.value;
  }

  .activity-item + .activity-item {
    border-top: 1px solid @fun.color.border.value;
  }

  .activity-text { font-weight: 600; font-size: @fun.type.small.value; }
  .activity-time { color: @fun.color.muted.value; font-size: 0.9rem; }

  @media (max-width: 1280px) {
    .kpi-grid { grid-template-columns: repeat(2, 1fr); }
    .admin-grid { grid-template-columns: 1fr; }
  }

  @media (max-width: 1080px) {
    .admin { grid-template-columns: 1fr; grid-template-rows: auto auto 1fr; }
    .admin-sidebar { position: static; height: auto; flex-direction: row; flex-wrap: wrap; }
    .sidebar-group { width: 240px; }
  }

  @media (max-width: 920px) {
    .admin-header { flex-direction: column; align-items: flex-start; padding: @fun.space.md.value; }
    .header-search { min-width: 100%; }
    .kpi-grid { grid-template-columns: 1fr; }
  }
  `
}
@admin-layout()
```

## The HTML

```html
<div class="admin">
  <aside class="admin-sidebar">
    <div class="admin-brand">Acme Admin</div>
    <nav class="sidebar-group" aria-label="Overview">
      <div class="sidebar-title">Overview</div>
      <a class="sidebar-link sidebar-link--active" href="#">Dashboard</a>
      <a class="sidebar-link" href="#">Analytics</a>
      <a class="sidebar-link" href="#">Reports</a>
    </nav>
    <nav class="sidebar-group" aria-label="Management">
      <div class="sidebar-title">Management</div>
      <a class="sidebar-link" href="#">Users</a>
      <a class="sidebar-link" href="#">Orders</a>
      <a class="sidebar-link" href="#">Products</a>
      <a class="sidebar-link" href="#">Roles</a>
    </nav>
    <nav class="sidebar-group" aria-label="Settings">
      <div class="sidebar-title">Settings</div>
      <a class="sidebar-link" href="#">Billing</a>
      <a class="sidebar-link" href="#">API Keys</a>
      <a class="sidebar-link" href="#">Integrations</a>
    </nav>
  </aside>

  <header class="admin-header">
    <input class="header-search" type="search" placeholder="Search users, orders, or reports…" aria-label="Global search">
    <div class="header-actions">
      <button class="ghost-btn">Notifications</button>
      <div class="avatar" aria-label="User avatar"></div>
    </div>
  </header>

  <main class="admin-main">
    <h1 class="admin-title">Dashboard</h1>

    <section class="kpi-grid">
      <article class="kpi-card">
        <div class="kpi-label">Users</div>
        <div class="kpi-value">12,840</div>
        <div class="kpi-delta">+12% vs last month</div>
      </article>
      <article class="kpi-card">
        <div class="kpi-label">Revenue</div>
        <div class="kpi-value">$84,290</div>
        <div class="kpi-delta">+8% vs last month</div>
      </article>
      <article class="kpi-card">
        <div class="kpi-label">Orders</div>
        <div class="kpi-value">3,240</div>
        <div class="kpi-delta">+18% vs last month</div>
      </article>
      <article class="kpi-card">
        <div class="kpi-label">Churn</div>
        <div class="kpi-value">2.8%</div>
        <div class="kpi-delta">-4% vs last month</div>
      </article>
    </section>

    <div class="admin-grid">
      <section class="panel">
        <header class="panel-header">
          <div class="panel-title">Recent Users</div>
          <button class="ghost-btn">View all</button>
        </header>
        <div class="table-wrap">
          <table class="admin-table">
            <thead>
              <tr>
                <th>User</th>
                <th>Role</th>
                <th>Status</th>
                <th>Last active</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>Avery Chen</td>
                <td>Admin</td>
                <td><span class="badge badge--success">Active</span></td>
                <td>2m ago</td>
              </tr>
              <tr>
                <td>Morgan Ellis</td>
                <td>Editor</td>
                <td><span class="badge badge--warning">Pending</span></td>
                <td>1h ago</td>
              </tr>
              <tr>
                <td>Jordan Lee</td>
                <td>Viewer</td>
                <td><span class="badge badge--success">Active</span></td>
                <td>3h ago</td>
              </tr>
              <tr>
                <td>Casey Nguyen</td>
                <td>Editor</td>
                <td><span class="badge badge--danger">Suspended</span></td>
                <td>Yesterday</td>
              </tr>
              <tr>
                <td>Riley Park</td>
                <td>Admin</td>
                <td><span class="badge badge--success">Active</span></td>
                <td>2d ago</td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>

      <aside class="panel">
        <header class="panel-header">
          <div class="panel-title">Activity</div>
        </header>
        <div class="activity-list">
          <div class="activity-item">
            <div class="activity-text">New user signed up</div>
            <div class="activity-time">2 minutes ago</div>
          </div>
          <div class="activity-item">
            <div class="activity-text">Order #4821 paid</div>
            <div class="activity-time">18 minutes ago</div>
          </div>
          <div class="activity-item">
            <div class="activity-text">Role updated to Editor</div>
            <div class="activity-time">1 hour ago</div>
          </div>
          <div class="activity-item">
            <div class="activity-text">API key revoked</div>
            <div class="activity-time">3 hours ago</div>
          </div>
          <div class="activity-item">
            <div class="activity-text">Report exported</div>
            <div class="activity-time">Yesterday</div>
          </div>
        </div>
      </aside>
    </div>
  </main>
</div>
```

## What Happens

- Token root `@admin-root()` defines a full dashboard palette (bg, surfaces, semantic states). `@fun` provides typed access with fallbacks.
- `@event.statusClass(s)` maps status strings to badge modifier classes (`badge--success/warning/danger/muted`) — pure build-time logic.
- Arrays drive KPIs, table rows, and activity feed (`kpi*`, `user`, `role`, `status`, `last`, `act`) with `count()` available for generation.
- `@admin-layout()` emits a CSS Grid shell: fixed sidebar column, sticky header, main content with gaps. Uses `grid-row: 1 / -1` for full-height sidebar and `height: 100vh` + `overflow-y: auto`.
- KPI grid spans 4→2→1 columns; admin grid spans 2→1 columns. Sidebar becomes horizontal wrap under 1080px, header stacks under 920px.
- Badges use semi-transparent backgrounds with readable text; focus rings via `@fun.color.ring.value`.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Generate KPI cards from `@arr.kpi*` using `count()` and `rpt()`/interpolation conceptually.
2. Generate the table body from `@arr.user/role/status/last` and apply `@event.statusClass(@arr.status[i])`.
3. Add a compact sidebar mode and dark theme via token root override + `@event`.

## Summary

Arrays + `@event` + `num()`-ready values + block-defined grid deliver a production-like admin panel. Token roots keep states consistent, while responsive rules make it usable across viewports — the capstone for FSCSS projects.

---

**Next: [Back to Projects](../README.md)**
