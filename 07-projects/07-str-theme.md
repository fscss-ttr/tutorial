# 07 — str & theme store

## Goal

Store a **reusable multi-line block** with `str()` and apply it to components.

## Skills

- `str(name, "…")` style store
- Variable interpolation `$name!` inside stores
- [str()](https://fscss.devtem.org/str)

## Deliverable

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  $primary: #3b82f6;
  $radius: 12px;
  $shadow: 0 10px 30px rgba(0,0,0,.25);

  str(card-style, "
    background: #121a30;
    color: #e6edf7;
    border-radius: $radius!;
    box-shadow: $shadow!;
    padding: 1.25rem;
    max-width: 320px;
  ")

  body {
    margin: 0;
    min-height: 100vh;
    display: grid;
    place-items: center;
    background: #0b1020;
    font-family: system-ui, sans-serif;
  }

  .card {
    card-style
    border-left: 4px solid $primary!;
  }
</style>
<div class="card">
  <strong>Theme store</strong>
  <p style="margin:0.5rem 0 0;opacity:.8">Powered by str()</p>
</div>
```

## Check

- Card uses stored padding/radius/shadow; left border uses `$primary`.

## Stretch

- Register a custom alias: `str(bgColor, "background-color")` and use `bgColor: …`.
- Skim camelCase / snake_case modules (same `str` mechanism).

## Next

[08 — st-core chart](./08-st-core-chart.md)
