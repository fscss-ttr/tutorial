# 10 — Dashboard compose

## Goal

Compose **st-core + overlapui** (and optional circle-progress) into one small dashboard sheet.

## Skills

- Multi-module `@import`
- Token roots coexistence (`--st-*` + `--ou-*`)
- Layout in the consumer sheet; modules own components only
- Module design: composition over mega-mixins

## Deliverable (skeleton)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>10 — Dashboard compose</title>
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
  <style>
    @import((*) from st-core@v2)
    @import((ou-root, avatar-overlap, ou-reduced-motion) from overlapui)

    @st-root()
    @ou-root()
    @avatar-overlap(.avatar-overlap)
    @ou-reduced-motion()

    @arr series[42, 55, 48, 70, 62, 80, 74]
    @st-chart-fill(.chart-fill, series)
    @st-chart-line(.chart-line, series)

    :root {
      --ou-size: 32px;
      --ou-overlap: -10px;
      --ou-ring: #0f172a;
    }

    body {
      margin: 0;
      min-height: 100vh;
      padding: 24px 16px;
      background: var(--st-bg);
      color: var(--st-text);
      font-family: system-ui, sans-serif;
    }

    .shell {
      max-width: 720px;
      margin: 0 auto;
      display: flex;
      flex-direction: column;
      gap: 16px;
    }

    .card {
      background: var(--st-card);
      border: 1px solid var(--st-border);
      border-radius: var(--st-radius-lg);
      padding: 16px;
    }

    .card h2 {
      margin: 0 0 12px;
      font-size: 12px;
      letter-spacing: 0.06em;
      text-transform: uppercase;
      color: var(--st-muted);
    }

    .chart {
      @st-chart-points(series)
      position: relative;
      height: 200px;
      border-radius: 12px;
      overflow: hidden;
      background: var(--st-surface);
    }

    .members {
      display: flex;
      align-items: center;
      gap: 12px;
    }
  </style>
</head>
<body>
  <div class="shell">
    <div class="card">
      <h2>Team</h2>
      <div class="members">
        <ul class="avatar-overlap" tabindex="0">
          <li data-alph="a">Ada</li>
          <li data-alph="n">Nia</li>
          <li data-alph="k">Ken</li>
        </ul>
        <span style="color:var(--st-muted);font-size:14px">Design pod · 3</span>
      </div>
    </div>
    <div class="card">
      <h2>Activity</h2>
      <div class="chart">
        <div class="chart-fill"></div>
        <div class="chart-line"></div>
      </div>
    </div>
  </div>
</body>
</html>
```

## Check

- Avatars and chart on one page; tokens don’t clash.
- Layout (`shell` / `card`) is yours; modules only style components.

## Stretch

- Add [circle-progress](https://github.com/Figsh/Circle-progress.fscss) KPI ring.
- CLI-compile to one CSS file for production.
- Publish a tiny `templates/dashboard` in your own module repo using [Module Design](../) practices.
- Optional: spider/radar pre-release from st-core when you track that branch.

## Done

You’ve gone from **runtime hello** → **language features** → **registry modules** → **composed UI**.

**Next learning paths**

- Write your own module ([Building a library](https://github.com/fscss-ttr))  
- [Pattern](https://fscss.devtem.org/pattern) · [Event](https://fscss.devtem.org/event) · [Hub](https://hub.devtem.org)
