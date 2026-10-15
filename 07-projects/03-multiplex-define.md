# 03 — Multiplex & define

## Goal

Reuse styles with **`%n` multiplexing** and a parameterized **`@define`**.

## Skills

- `%2` / `%4` / `$list[: value;]`
- `@define` · `@use(param)`
- [Define](https://fscss.devtem.org/define) · [Loops — %n](https://fscss.devtem.org/loop)

## Deliverable

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  @arr box[width, height]
  $box: @arr.box!;

  @define panel(bg: #12121a, accent: #9d7eff) {
    background: @use(bg);
    border-left: 4px solid @use(accent);
    color: #e8e3ff;
    padding: 1rem 1.25rem;
    border-radius: 12px;
  }

  body {
    margin: 0;
    min-height: 100vh;
    display: grid;
    place-items: center;
    background: #0e0d14;
    font-family: system-ui, sans-serif;
  }

  .tile {
    @panel(#1c1a2e, #4fffb0)
    %2($box.list[: 160px;])
  }
</style>
<div class="tile">Define + %2</div>
```

## Check

- Tile is 160×160 with green accent bar.
- Changing `@panel(…)` args restyles without editing the rule body.

## Stretch

- Block define with backticks for a `@media` helper.
- Import nothing yet—keep pure core language.

## Next

[04 — Array stagger](./04-array-stagger.md)
