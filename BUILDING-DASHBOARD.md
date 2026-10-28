# Build an AI Ops Dashboard with FSCSS

**ai-ui · st-core@v2 · circle-progress · overlapui**

A pure-CSS presentation layer for AI product surfaces, plus statistical charts, circular progress, and overlapping avatars — compiled with the FSCSS CLI and wired with a small JavaScript layer.

This tutorial walks through a **production-friendly split**:

```
ai-dashboard/
├── dashboard.html      # markup only
├── dashboard.fscss     # all FSCSS + layout
├── dashboard.css       # compiled output (generated)
└── dashboard.js        # chat, drawers, chart updates
```

No runtime preprocessor in the browser for production. You compile once, ship plain CSS.

---

## What you will build

An **AI Ops Console** with:

| Surface | Module |
|---------|--------|
| App shell, sidebar, messages, prompt, stream caret, thinking dots, usage cards, workflow nodes, tool chips, citations | **ai-ui.fscss** |
| Token burn area/line chart (clip-path, no SVG/canvas) | **st-core@v2** |
| GPU / Queue / Quota rings | **circle-progress** |
| Team avatar stack | **overlapui** |
| Mock agent replies, live chart nudges, mobile drawers | **dashboard.js** |

Desktop: sidebar + chat + metrics panel.  
Mobile (≤960px): hamburger sidebar drawer + bottom-sheet metrics panel.

---

## Prerequisites

- **Node.js** 18+ (for the FSCSS CLI)
- A modern browser
- Optional: VS Code + [FSCSS Support](https://marketplace.visualstudio.com/items?itemName=Figsh.fscss) extension

Install the compiler globally:

```bash
npm install -g fscss@1.2.6
```

Requires **FSCSS ≥ 1.2.6** (ai-ui needs it; st-core needs ≥ 1.2.3).

---

## 1. Project layout

```bash
mkdir ai-dashboard && cd ai-dashboard
touch dashboard.html dashboard.fscss dashboard.js
```

You will generate `dashboard.css` with the CLI.

---

## 2. `dashboard.fscss` — styles source

This file imports the four modules, applies composition helpers, defines the chart series, and holds all layout + responsive rules.

```css
/* dashboard.fscss
   AI Ops Dashboard — FSCSS source
   Compile:  fscss dashboard.fscss dashboard.css
*/

/* ── Modules (registry names; CLI + runtime resolve these) ── */
@import((*) from ai-ui)
@import((*) from st-core@v2)
@import((*) from circle-progress)
@import((*) from overlapui)

/* ── Theme + design tokens ── */
@ai-compose-dashboard()   /* ai-ui tokens + dashboard theme */
@st-root()                /* st-core chart tokens */
@progress-root()          /* circle-progress tokens */
@ou-root()                /* overlapui tokens */

/* ── ai-ui structure helpers ── */
@ai-shell(.ai-shell)
@ai-sidebar(.ai-sidebar)
@ai-main(.ai-main)
@ai-messages(.ai-messages)
@ai-message(.ai-message)
@ai-message-user(.ai-message-user)
@ai-message-assistant(.ai-message-assistant)
@ai-bubble(.ai-bubble)
@ai-avatar(.ai-avatar)
@ai-prompt(.ai-prompt)
@ai-prompt-box(.ai-prompt-box)
@ai-prompt-send(.ai-prompt-send)
@ai-stream(.ai-stream)
@ai-thinking(.ai-thinking)
@ai-usage(.ai-usage)
@ai-stat(.ai-stat)
@ai-workflow(.ai-workflow)
@ai-node(.ai-node)
@ai-edge(.ai-edge)
@ai-tools(.ai-tools)
@ai-tool-chip(.ai-tool-chip)
@ai-card(.ai-card)
@ai-divider(.ai-divider)
@ai-citations(.ai-citations)
@ai-citation(.ai-citation)

/* ── st-core linear chart ── */
@arr tokenSeries[32, 48, 55, 42, 68, 75, 61, 88, 72, 91, 84, 96]

@st-chart-fill(.chart-fill, tokenSeries)
@st-chart-line(.chart-line, tokenSeries)

.token-chart {
  @st-chart-points(tokenSeries)
  position: relative;
  height: 160px;
  width: 100%;
  background: var(--st-surface, #161422);
  border-radius: 12px;
  overflow: hidden;
}

/* ── circle-progress ── */
@circle-progress(.cp)
.cp-72 { @progress-range(72) }
.cp-48 { @progress-range(48) }
.cp-91 { @progress-range(91) }

.cp {
  --progress-size: 110px;
  --progress-stroke: 10px;
  --progress-color-bg: transparent;
  --progress-color-track: rgba(255, 255, 255, 0.08);
  --progress-color-arc: var(--ai-accent, #e879f9);
  --progress-color-glow: color-mix(in srgb, var(--ai-accent, #e879f9) 40%, transparent);
  --progress-color-value: var(--ai-text, #f5f3ff);
  --progress-color-label: var(--ai-text-muted, #a78bfa);
  font-size: 0.95rem;
  font-weight: 600;
}

/* ── overlapui avatars ── */
@avatar-overlap(.team-faces)
.team-faces {
  --ou-avatar-size: 36px;
  --ou-overlap: -12px;
  margin-block: 8px;
}

/* ── Base layout ── */
* { box-sizing: border-box; margin: 0; padding: 0; }

html, body {
  height: 100%;
  font-family: var(--ai-font, system-ui, -apple-system, sans-serif);
  background: var(--ai-bg, #0c0a12);
  color: var(--ai-text, #f5f3ff);
  -webkit-text-size-adjust: 100%;
}

.ai-shell {
  min-height: 100vh;
  min-height: 100dvh;
  height: 100vh;
  height: 100dvh;
  display: grid;
  grid-template-columns: var(--ai-sidebar-w, 240px) 1fr;
}

.ai-sidebar {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 20px 16px;
  overflow-y: auto;
  border-right: 1px solid var(--ai-border, rgba(255, 255, 255, 0.08));
  background: var(--ai-bg, #0c0a12);
  z-index: 40;
}

.brand {
  display: flex;
  align-items: center;
  gap: 10px;
  font-weight: 700;
  font-size: 1.05rem;
  letter-spacing: -0.02em;
}
.brand-mark {
  width: 28px;
  height: 28px;
  border-radius: 8px;
  background: linear-gradient(135deg, var(--ai-accent, #e879f9), #818cf8);
  display: grid;
  place-items: center;
  font-size: 0.75rem;
  font-weight: 800;
  color: #0c0a12;
  flex-shrink: 0;
}

.nav {
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.nav a {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 12px;
  border-radius: 8px;
  color: var(--ai-text-muted, #a78bfa);
  text-decoration: none;
  font-size: 0.875rem;
  transition: background 0.15s, color 0.15s;
}
.nav a:hover,
.nav a.active {
  background: color-mix(in srgb, var(--ai-accent, #e879f9) 12%, transparent);
  color: var(--ai-text, #f5f3ff);
}

.side-section {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.side-section h3 {
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  color: var(--ai-text-muted, #a78bfa);
  font-weight: 600;
}

.cp-row {
  display: flex;
  justify-content: space-between;
  gap: 8px;
  flex-wrap: wrap;
}
.cp-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  font-size: 0.7rem;
  color: var(--ai-text-muted, #a78bfa);
}

.ai-main {
  display: flex;
  flex-direction: column;
  min-height: 0;
  min-width: 0;
  overflow: hidden;
}

.topbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  border-bottom: 1px solid var(--ai-border, rgba(255, 255, 255, 0.08));
  gap: 10px;
  flex-wrap: wrap;
  flex-shrink: 0;
}
.topbar h1 {
  font-size: 1rem;
  font-weight: 600;
}
.topbar-actions {
  display: flex;
  gap: 8px;
  align-items: center;
  flex-wrap: wrap;
}
.chip {
  font-size: 0.75rem;
  padding: 4px 10px;
  border-radius: 999px;
  background: color-mix(in srgb, var(--ai-accent, #e879f9) 15%, transparent);
  color: var(--ai-accent, #e879f9);
  border: 1px solid color-mix(in srgb, var(--ai-accent, #e879f9) 30%, transparent);
  white-space: nowrap;
}

.workspace {
  flex: 1;
  display: grid;
  grid-template-columns: 1fr minmax(260px, 320px);
  min-height: 0;
  overflow: hidden;
}

.chat-col {
  display: flex;
  flex-direction: column;
  min-height: 0;
  min-width: 0;
  border-right: 1px solid var(--ai-border, rgba(255, 255, 255, 0.06));
}

.ai-messages {
  flex: 1;
  overflow-y: auto;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 14px;
  -webkit-overflow-scrolling: touch;
}

.ai-prompt {
  padding: 12px 16px calc(16px + env(safe-area-inset-bottom, 0px));
  border-top: 1px solid var(--ai-border, rgba(255, 255, 255, 0.06));
  flex-shrink: 0;
}

.right-panel {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 16px;
  overflow-y: auto;
  background: color-mix(in srgb, var(--ai-surface, #12101a) 80%, transparent);
  -webkit-overflow-scrolling: touch;
}

.panel-card {
  background: var(--ai-card, #1a1625);
  border: 1px solid var(--ai-border, rgba(255, 255, 255, 0.08));
  border-radius: 12px;
  padding: 14px;
}
.panel-card h3 {
  font-size: 0.8rem;
  font-weight: 600;
  margin-bottom: 10px;
  color: var(--ai-text-muted, #a78bfa);
}

.ai-workflow {
  display: flex;
  align-items: center;
  gap: 6px;
  flex-wrap: wrap;
  font-size: 0.75rem;
}
.ai-node {
  padding: 6px 10px;
  border-radius: 6px;
  background: var(--ai-surface, #161422);
  border: 1px solid var(--ai-border, rgba(255, 255, 255, 0.1));
}
.ai-node.is-active {
  border-color: var(--ai-accent, #e879f9);
  color: var(--ai-accent, #e879f9);
  box-shadow: 0 0 0 1px color-mix(in srgb, var(--ai-accent, #e879f9) 40%, transparent);
}
.ai-edge {
  width: 16px;
  height: 2px;
  background: var(--ai-border, rgba(255, 255, 255, 0.2));
}

.ai-usage {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}
.ai-stat {
  background: var(--ai-surface, #161422);
  border-radius: 10px;
  padding: 10px 12px;
}
.ai-stat-label {
  display: block;
  font-size: 0.65rem;
  text-transform: uppercase;
  letter-spacing: 0.04em;
  color: var(--ai-text-muted, #a78bfa);
  margin-bottom: 4px;
}
.ai-stat-value {
  font-size: 1.15rem;
  font-weight: 700;
  letter-spacing: -0.02em;
}
.ai-stat-delta {
  font-size: 0.7rem;
  margin-left: 6px;
}
.ai-stat-delta.up { color: #4fffb0; }
.ai-stat-delta.down { color: #ff5e7d; }

.ai-tools {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}
.ai-tool-chip {
  font-size: 0.7rem;
  padding: 4px 8px;
  border-radius: 6px;
  background: var(--ai-surface, #161422);
  border: 1px solid var(--ai-border, rgba(255, 255, 255, 0.1));
}
.ai-tool-chip.is-done { border-color: #4fffb0; color: #4fffb0; }
.ai-tool-chip.is-running { border-color: var(--ai-accent, #e879f9); color: var(--ai-accent, #e879f9); }

.muted {
  color: var(--ai-text-muted, #a78bfa);
  font-size: 0.8rem;
}

/* Mobile controls (hidden on desktop) */
.menu-btn,
.panel-btn,
.sidebar-backdrop {
  display: none;
}

/* ═══════════════ TABLET ≤ 960px ═══════════════ */
@media (max-width: 960px) {
  .ai-shell {
    grid-template-columns: 1fr;
  }

  .ai-sidebar {
    position: fixed;
    inset: 0 auto 0 0;
    width: min(280px, 86vw);
    transform: translateX(-105%);
    transition: transform 0.25s ease;
    box-shadow: none;
    border-right: 1px solid var(--ai-border, rgba(255, 255, 255, 0.08));
  }
  body.sidebar-open .ai-sidebar {
    transform: translateX(0);
    box-shadow: 8px 0 32px rgba(0, 0, 0, 0.45);
  }

  .sidebar-backdrop {
    display: block;
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.5);
    z-index: 35;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.25s ease;
  }
  body.sidebar-open .sidebar-backdrop {
    opacity: 1;
    pointer-events: auto;
  }

  .menu-btn,
  .panel-btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 36px;
    height: 36px;
    border-radius: 8px;
    border: 1px solid var(--ai-border, rgba(255, 255, 255, 0.12));
    background: var(--ai-surface, #161422);
    color: var(--ai-text, #f5f3ff);
    cursor: pointer;
    font-size: 1.1rem;
    flex-shrink: 0;
    line-height: 1;
  }

  .topbar {
    padding: 10px 12px;
    gap: 8px;
  }
  .topbar h1 {
    font-size: 0.95rem;
    flex: 1;
    min-width: 0;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .topbar-actions { gap: 6px; }
  .chip {
    font-size: 0.7rem;
    padding: 3px 8px;
  }
  #model-chip { display: none; }

  .workspace {
    grid-template-columns: 1fr;
    grid-template-rows: 1fr;
    position: relative;
  }
  .chat-col {
    border-right: none;
    min-height: 0;
  }

  .right-panel {
    position: fixed;
    inset: auto 0 0 0;
    max-height: min(72vh, 560px);
    border-radius: 16px 16px 0 0;
    border-top: 1px solid var(--ai-border, rgba(255, 255, 255, 0.1));
    transform: translateY(110%);
    transition: transform 0.28s ease;
    z-index: 30;
    box-shadow: 0 -8px 32px rgba(0, 0, 0, 0.4);
    padding-bottom: calc(16px + env(safe-area-inset-bottom, 0px));
  }
  body.panel-open .right-panel {
    transform: translateY(0);
  }
  .right-panel::before {
    content: "";
    display: block;
    width: 36px;
    height: 4px;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.2);
    margin: 0 auto 10px;
  }

  .token-chart { height: 140px; }
  .cp {
    --progress-size: 88px;
    --progress-stroke: 8px;
    font-size: 0.85rem;
  }
}

/* ═══════════════ PHONE ≤ 600px ═══════════════ */
@media (max-width: 600px) {
  .ai-messages {
    padding: 12px;
    gap: 12px;
  }
  .ai-prompt {
    padding: 10px 12px calc(12px + env(safe-area-inset-bottom, 0px));
  }
  .ai-usage {
    grid-template-columns: 1fr 1fr;
    gap: 8px;
  }
  .ai-stat { padding: 8px 10px; }
  .ai-stat-value { font-size: 1rem; }

  .cp-row {
    justify-content: center;
    gap: 12px;
  }
  .cp {
    --progress-size: 76px;
    --progress-stroke: 7px;
    font-size: 0.8rem;
  }

  .panel-card { padding: 12px; }
  .ai-workflow { gap: 4px; }
  .ai-node {
    padding: 5px 8px;
    font-size: 0.7rem;
  }
  .ai-edge { width: 10px; }

  .team-faces {
    --ou-avatar-size: 32px;
    --ou-overlap: -10px;
  }
  .ai-bubble { font-size: 0.9rem; }
}

/* ═══════════════ VERY NARROW ≤ 380px ═══════════════ */
@media (max-width: 380px) {
  .ai-usage { grid-template-columns: 1fr; }
  .chip#btn-refresh {
    padding: 3px 6px;
    font-size: 0.65rem;
  }
}
```

### Why this structure works

1. **Import** only registers `@define` helpers — nothing paints until you call them.
2. **Compose / root** helpers write design tokens on `:root` (or a scope).
3. **Selector-parameterized helpers** (`@ai-shell(.ai-shell)`, `@st-chart-fill(.chart-fill, tokenSeries)`, …) attach rules to *your* class names.
4. Plain CSS after that is normal layout, overrides, and media queries.

If the CLI cannot resolve registry names offline, use remote URLs instead:

```fscss
@import((*) from "https://cdn.jsdelivr.net/gh/fscss-ttr/ai-ui.fscss@main/ai-ui.fscss")
@import((*) from "https://cdn.jsdelivr.net/gh/fscss-ttr/st-core.fscss@main/st-core@v2.fscss")
@import((*) from "https://cdn.jsdelivr.net/gh/fscss-ttr/FSCSS@main/xf/styles/circle-progress.fscss")
@import((*) from "https://cdn.jsdelivr.net/gh/fscss-ttr/FSCSS@main/xf/styles/overlapui.fscss")
```

---

## 3. Compile FSCSS → CSS

```bash
# one-shot
fscss dashboard.fscss dashboard.css

# watch (recompile on save) — if your fscss build supports -w / watch
# fscss dashboard.fscss dashboard.css --watch
```

What the compiler does:

- Expands every `@import` / `@define` / `@arr` into standard CSS
- Emits custom properties (`--st-p1` … `--st-p12`, `--ai-*`, `--progress-*`, …)
- Emits `clip-path: polygon(...)` for the chart fill and line
- Leaves your media queries and plain CSS intact

Ship **only** `dashboard.css` to production. No `runtime.min.js` required for this path.

---

## 4. `dashboard.html` — markup

Link the **compiled** stylesheet. No FSCSS runtime script.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover" />
  <title>AI Ops Dashboard · ai-ui + st-core@v2</title>
  <link rel="stylesheet" href="dashboard.css" />
</head>
<body>
  <div class="sidebar-backdrop" id="sidebar-backdrop" aria-hidden="true"></div>

  <div class="ai-shell">
    <!-- Sidebar -->
    <aside class="ai-sidebar" id="sidebar" aria-label="Sidebar">
      <div class="brand">
        <span class="brand-mark">AI</span>
        <span>Ops Console</span>
      </div>

      <nav class="nav">
        <a href="#" class="active">Dashboard</a>
        <a href="#">Agents</a>
        <a href="#">Workflows</a>
        <a href="#">Usage</a>
        <a href="#">Settings</a>
      </nav>

      <div class="side-section">
        <h3>Team online</h3>
        <div class="team-faces avatar-overlap">
          <div class="ou-avatar" data-alph="F" style="--ou-c: #e879f9"></div>
          <div class="ou-avatar" data-alph="S" style="--ou-c: #818cf8"></div>
          <div class="ou-avatar" data-alph="A" style="--ou-c: #4fffb0"></div>
          <div class="ou-avatar" data-alph="+" style="--ou-c: #fbbf24"></div>
        </div>
        <p class="muted">4 agents · 2 human</p>
      </div>

      <div class="side-section">
        <h3>Capacity</h3>
        <div class="cp-row">
          <div class="cp-item">
            <div class="cp cp-72">72%</div>
            <span>GPU</span>
          </div>
          <div class="cp-item">
            <div class="cp cp-48">48%</div>
            <span>Queue</span>
          </div>
          <div class="cp-item">
            <div class="cp cp-91">91%</div>
            <span>Quota</span>
          </div>
        </div>
      </div>

      <div class="side-section" style="margin-top: auto;">
        <h3>Session</h3>
        <p class="muted" id="session-info">Compiled · FSCSS 1.2.6</p>
      </div>
    </aside>

    <!-- Main -->
    <main class="ai-main">
      <header class="topbar">
        <button type="button" class="menu-btn" id="menu-btn" aria-label="Open menu" aria-expanded="false">☰</button>
        <h1>Agent workspace</h1>
        <div class="topbar-actions">
          <span class="chip" id="model-chip">model: gpt-ops · live</span>
          <button type="button" class="chip" id="btn-refresh" style="cursor:pointer;border:none;">↻ refresh</button>
          <button type="button" class="panel-btn" id="panel-btn" aria-label="Open metrics panel" aria-expanded="false">▣</button>
        </div>
      </header>

      <div class="workspace">
        <div class="chat-col">
          <div class="ai-messages" id="messages">
            <div class="ai-message ai-message-assistant">
              <div class="ai-avatar">AI</div>
              <div class="ai-bubble">
                Ops console ready. Token burn and queue depth are within targets.
                Ask for a status summary or kick off a workflow.
              </div>
            </div>
            <div class="ai-message ai-message-user">
              <div class="ai-avatar">U</div>
              <div class="ai-bubble">Show token trend and current agent pipeline.</div>
            </div>
            <div class="ai-message ai-message-assistant">
              <div class="ai-avatar">AI</div>
              <div class="ai-bubble">
                <div class="ai-tools" style="margin-bottom:8px">
                  <span class="ai-tool-chip is-done">✓ metrics.read</span>
                  <span class="ai-tool-chip is-done">✓ workflow.snapshot</span>
                </div>
                Token series is up ~12% vs yesterday. Pipeline is on step <strong>Generate</strong>.
                Chart and workflow updated in the side panel.
                <div class="ai-citations" style="margin-top:8px">
                  <a class="ai-citation" href="#"><span class="ai-cite-num">1</span> metrics.api</a>
                  <a class="ai-citation" href="#"><span class="ai-cite-num">2</span> agent.graph</a>
                </div>
              </div>
            </div>
          </div>

          <form class="ai-prompt" id="prompt-form">
            <textarea class="ai-prompt-box" id="prompt-input" rows="1" placeholder="Ask the ops agent…" aria-label="Prompt"></textarea>
            <button class="ai-prompt-send" type="submit" aria-label="Send">↑</button>
          </form>
        </div>

        <aside class="right-panel" id="right-panel" aria-label="Metrics panel">
          <div class="panel-card">
            <h3>Token burn (12 pts)</h3>
            <div class="token-chart" id="token-chart">
              <div class="chart-fill"></div>
              <div class="chart-line"></div>
            </div>
          </div>

          <div class="panel-card">
            <h3>Agent pipeline</h3>
            <div class="ai-workflow" id="workflow">
              <div class="ai-node">Ingest</div>
              <div class="ai-edge"></div>
              <div class="ai-node">Parse</div>
              <div class="ai-edge"></div>
              <div class="ai-node is-active">Generate</div>
              <div class="ai-edge"></div>
              <div class="ai-node">Review</div>
            </div>
          </div>

          <div class="panel-card">
            <h3>Usage today</h3>
            <div class="ai-usage">
              <div class="ai-stat">
                <span class="ai-stat-label">Tokens</span>
                <span class="ai-stat-value" id="stat-tokens">48.2k</span>
                <span class="ai-stat-delta up">+12%</span>
              </div>
              <div class="ai-stat">
                <span class="ai-stat-label">Requests</span>
                <span class="ai-stat-value" id="stat-req">1,284</span>
                <span class="ai-stat-delta up">+4%</span>
              </div>
              <div class="ai-stat">
                <span class="ai-stat-label">Latency p95</span>
                <span class="ai-stat-value" id="stat-lat">312ms</span>
                <span class="ai-stat-delta down">−8%</span>
              </div>
              <div class="ai-stat">
                <span class="ai-stat-label">Errors</span>
                <span class="ai-stat-value" id="stat-err">0.4%</span>
                <span class="ai-stat-delta down">−0.1</span>
              </div>
            </div>
          </div>

          <div class="panel-card">
            <h3>Active tools</h3>
            <div class="ai-tools" id="tools">
              <span class="ai-tool-chip is-done">✓ Search</span>
              <span class="ai-tool-chip is-running">● Stream</span>
              <span class="ai-tool-chip">Code exec</span>
              <span class="ai-tool-chip">RAG</span>
            </div>
          </div>
        </aside>
      </div>
    </main>
  </div>

  <script src="dashboard.js" defer></script>
</body>
</html>
```

Class names match the selectors you passed into the FSCSS helpers (`@ai-shell(.ai-shell)`, `@circle-progress(.cp)`, …).

---

## 5. `dashboard.js` — behavior

```js
// dashboard.js
// Chat mock, mobile drawers, st-core live chart updates

const $ = (s, r = document) => r.querySelector(s);
const $$ = (s, r = document) => [...r.querySelectorAll(s)];

/* ── st-core: update linear chart via CSS variables ──
   Higher data value → lower --st-pN % → higher visual position.
*/
function updateTokenChart(values) {
  const el = $('#token-chart');
  if (!el) return;
  el.style.cssText = values
    .map((v, i) => `--st-p${i + 1}: ${100 - v}%`)
    .join('; ');
}

let series = [32, 48, 55, 42, 68, 75, 61, 88, 72, 91, 84, 96];

function nudgeSeries() {
  series = series.map((v) => {
    const d = (Math.random() - 0.45) * 12;
    return Math.max(8, Math.min(98, Math.round(v + d)));
  });
  updateTokenChart(series);
  const sum = series.reduce((a, b) => a + b, 0);
  const tokensEl = $('#stat-tokens');
  if (tokensEl) tokensEl.textContent = (sum * 0.42).toFixed(1) + 'k';
}

/* ── Mobile drawers ── */
const menuBtn = $('#menu-btn');
const panelBtn = $('#panel-btn');
const backdrop = $('#sidebar-backdrop');

function closeSidebar() {
  document.body.classList.remove('sidebar-open');
  menuBtn?.setAttribute('aria-expanded', 'false');
}
function openSidebar() {
  document.body.classList.add('sidebar-open');
  document.body.classList.remove('panel-open');
  menuBtn?.setAttribute('aria-expanded', 'true');
  panelBtn?.setAttribute('aria-expanded', 'false');
}
function closePanel() {
  document.body.classList.remove('panel-open');
  panelBtn?.setAttribute('aria-expanded', 'false');
}
function openPanel() {
  document.body.classList.add('panel-open');
  document.body.classList.remove('sidebar-open');
  panelBtn?.setAttribute('aria-expanded', 'true');
  menuBtn?.setAttribute('aria-expanded', 'false');
}

menuBtn?.addEventListener('click', () => {
  document.body.classList.contains('sidebar-open') ? closeSidebar() : openSidebar();
});
panelBtn?.addEventListener('click', () => {
  document.body.classList.contains('panel-open') ? closePanel() : openPanel();
});
backdrop?.addEventListener('click', () => {
  closeSidebar();
  closePanel();
});

const mq = window.matchMedia('(min-width: 961px)');
mq.addEventListener?.('change', (e) => {
  if (e.matches) {
    closeSidebar();
    closePanel();
  }
});

document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') {
    closeSidebar();
    closePanel();
  }
});

/* ── Chat / prompt ── */
const messages = $('#messages');
const form = $('#prompt-form');
const input = $('#prompt-input');

function appendMessage(role, html, streaming = false) {
  const row = document.createElement('div');
  row.className = `ai-message ai-message-${role}`;
  row.innerHTML = `
    <div class="ai-avatar">${role === 'user' ? 'U' : 'AI'}</div>
    <div class="ai-bubble${streaming ? ' ai-stream' : ''}">${html}</div>
  `;
  messages.appendChild(row);
  messages.scrollTop = messages.scrollHeight;
  return row;
}

function thinking() {
  const t = document.createElement('div');
  t.className = 'ai-message ai-message-assistant';
  t.id = 'thinking-row';
  t.innerHTML = `
    <div class="ai-avatar">AI</div>
    <div class="ai-bubble">
      <div class="ai-thinking">
        <span class="ai-dot"></span>
        <span class="ai-dot"></span>
        <span class="ai-dot"></span>
        Thinking…
      </div>
    </div>
  `;
  messages.appendChild(t);
  messages.scrollTop = messages.scrollHeight;
}

function clearThinking() {
  $('#thinking-row')?.remove();
}

const replies = [
  {
    tools: true,
    text: 'Pipeline advanced. Generate step is now complete; Review is active. Token chart refreshed.',
    advance: true,
  },
  {
    tools: false,
    text: 'Queue depth is healthy. GPU at ~72%. No action required unless you want to scale workers.',
    advance: false,
  },
  {
    tools: true,
    text: 'Pulled latest metrics. p95 latency improved. I can open a usage breakdown if you need it.',
    advance: false,
  },
];
let replyIdx = 0;

form?.addEventListener('submit', (e) => {
  e.preventDefault();
  const text = input.value.trim();
  if (!text) return;
  appendMessage('user', text);
  input.value = '';
  input.style.height = 'auto';

  thinking();
  setTimeout(() => {
    clearThinking();
    const r = replies[replyIdx % replies.length];
    replyIdx++;
    let body = '';
    if (r.tools) {
      body += `<div class="ai-tools" style="margin-bottom:8px">
        <span class="ai-tool-chip is-done">✓ metrics.read</span>
        <span class="ai-tool-chip is-running">● workflow.tick</span>
      </div>`;
    }
    body += r.text;
    const row = appendMessage('assistant', body, true);
    setTimeout(() => row.querySelector('.ai-bubble')?.classList.remove('ai-stream'), 1800);

    if (r.advance) {
      const nodes = $$('#workflow .ai-node');
      const active = nodes.findIndex((n) => n.classList.contains('is-active'));
      if (active >= 0 && active < nodes.length - 1) {
        nodes[active].classList.remove('is-active');
        nodes[active + 1].classList.add('is-active');
      }
    }
    nudgeSeries();
  }, 900 + Math.random() * 600);
});

input?.addEventListener('input', () => {
  input.style.height = 'auto';
  input.style.height = Math.min(input.scrollHeight, 120) + 'px';
});

$('#btn-refresh')?.addEventListener('click', () => {
  nudgeSeries();
  const btn = $('#btn-refresh');
  btn.textContent = '↻ updated';
  setTimeout(() => {
    btn.textContent = '↻ refresh';
  }, 1200);
});

updateTokenChart(series);
setInterval(nudgeSeries, 14000);
```

### Chart control layer (important)

st-core paints with CSS only. Live data means **setting custom properties** on the chart host:

```js
el.style.cssText = values
  .map((v, i) => `--st-p${i + 1}: ${100 - v}%`)
  .join('; ');
```

The polygon length is fixed at compile time from `@arr tokenSeries[...]` length (12 points here). Do not change the *number* of points at runtime without a matching compiled sheet; only change the *values*.

---

## 6. Run it

```bash
# compile
fscss dashboard.fscss dashboard.css

# serve (any static server)
npx serve .
# or: python -m http.server 8080
```

Open `http://localhost:3000/dashboard.html` (or your port).

**Checklist**

- [ ] Sidebar + brand + nav  
- [ ] Circle progress rings  
- [ ] Overlapping avatars  
- [ ] Token chart visible  
- [ ] Prompt sends → thinking → reply  
- [ ] ↻ refresh updates chart numbers  
- [ ] ≤960px: ☰ opens sidebar, ▣ opens bottom sheet  

---

## 7. How the modules fit

```
┌─────────────────────────────────────────────────────────┐
│  ai-ui.fscss                                            │
│  Shell · Messages · Prompt · Stream · Thinking · Usage  │
│  Workflow · Tools · Citations                           │
├──────────────────────┬──────────────────────────────────┤
│  st-core@v2          │  circle-progress                 │
│  Area / line chart   │  GPU · Queue · Quota rings       │
│  --st-pN variables   │  --progress-* variables          │
├──────────────────────┴──────────────────────────────────┤
│  overlapui                                              │
│  .team-faces avatar stack                               │
├─────────────────────────────────────────────────────────┤
│  dashboard.js                                           │
│  Drawers · chat mock · updateTokenChart(series)         │
└─────────────────────────────────────────────────────────┘
```

ai-ui and st-core intentionally avoid selector collisions so you can compose them (documented in the ai-ui README as “depth with st-core charts”).

---

## 8. Optional: npm scripts / CI

`package.json`:

```json
{
  "name": "ai-ops-dashboard",
  "private": true,
  "scripts": {
    "build:css": "fscss dashboard.fscss dashboard.css",
    "build": "npm run build:css",
    "start": "npx serve ."
  },
  "devDependencies": {
    "fscss": "1.2.6"
  }
}
```

```bash
npm install
npm run build
npm start
```

Pin `fscss@1.2.6` so local and CI match.

---

## 9. Prototype vs production

| Mode | When | How |
|------|------|-----|
| **Runtime (CDN)** | Quick demos, FSCSS IDE, single HTML file | `<script src="…/runtime.min.js">` + `@import` inside `<style>` |
| **CLI compile** | Real apps, static hosting, zero client preprocessor | `fscss in.fscss out.css` + `<link rel="stylesheet" href="out.css">` |

This tutorial uses the **CLI path**. Keep a single-file runtime version only for playgrounds.

---

## 10. Next steps

- Swap `@ai-compose-dashboard()` for `@ai-compose-copilot()` / `@ai-compose-research()`  
- Add a second `@arr` series + another fill/line for multi-line charts  
- Replace mock `replies` with a real agent / streaming API  
- Drive circle progress from JS by setting `--progress-*` or swapping `.cp-72` classes  
- Add a spider chart from st-core (`@st-spider-points`, `@st-spider-fill`, …) in a new panel card  

---

**[CodePen Preview](https://codepen.io/editor/David-Hux/pen/01a181f1-e94a-7029-9f5f-894ea531b2fc)**


## References

- [ai-ui.fscss](https://github.com/fscss-ttr/ai-ui.fscss) — AI chrome (shell, messages, prompt, usage, workflow)  
- [st-core.fscss](https://github.com/fscss-ttr/st-core.fscss) — pure CSS area / line / spider charts  
- [circle-progress](https://github.com/Figsh/Circle-progress.fscss) — conic-gradient rings  
- [overlapui](https://github.com/fscss-ttr/overlapui.fscss) — stacked avatars / cards  
- [FSCSS modules](https://github.com/fscss-ttr/fscss-modules) — import system  
- [fscss npm](https://www.npmjs.com/package/fscss) — CLI + runtime  

**MIT** modules · **FSCSS 1.2.6+**
