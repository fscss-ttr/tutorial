# 08 — st-core chart

## Goal

Import **st-core@v2** from the registry and render a pure CSS line/area chart.

## Skills

- Registry `@import`
- `@arr` data + `@st-chart-*`
- [st-core](https://github.com/fscss-ttr/st-core.fscss) · [Registry imports](../) · [Builder](https://hub.devtem.org/st-core.fscss/visual-builder/)

## Deliverable

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>08 — st-core chart</title>
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
  <style>
    @import((*) from st-core@v2)

    @st-root()

    @arr myData[56, 67, 70, 43, 67, 80, 55]

    @st-chart-fill(.chart-fill, myData)
    @st-chart-line(.chart-line, myData)

    body {
      margin: 0;
      min-height: 100vh;
      display: grid;
      place-items: center;
      background: var(--st-bg);
      font-family: system-ui, sans-serif;
    }

    .chart {
      @st-chart-points(myData)
      position: relative;
      height: 220px;
      width: min(520px, 92vw);
      background: var(--st-surface);
      border-radius: 12px;
      overflow: hidden;
    }

    .chart-fill, .chart-line {
      transition: clip-path 0.5s cubic-bezier(0.4, 0, 0.2, 1);
    }
  </style>
</head>
<body>
  <div class="chart">
    <div class="chart-fill"></div>
    <div class="chart-line"></div>
  </div>
</body>
</html>
```

## Check

- Area + line render from seven points.
- Changing `@arr myData[…]` changes the shape after reload.

## Stretch

- Update points from JS: set `--st-p1`, `--st-p2`, … on `.chart`.
- Try the [visual builder](https://hub.devtem.org/st-core.fscss/visual-builder/), then paste export.
- Pre-release spider: polar mixins when available on your branch.

## Next

[09 — overlapui](./09-overlapui.md)
