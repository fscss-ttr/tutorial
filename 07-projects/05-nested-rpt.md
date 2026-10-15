# 05 — Nested rpt

## Goal

Generate **nested selector chains** with `rpt()` and a color array.

## Skills

- `rpt(level, "div ")`
- Parallel `@arr` lookup
- [Nested loop](https://fscss.devtem.org/nested-loop) · [Loops](https://fscss.devtem.org/loop)

## Deliverable

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  @arr colors[orange, purple, indigo, #3b82f6]
  @arr levels[count(@arr.colors!.length)]

  body {
    margin: 0;
    min-height: 100vh;
    display: grid;
    place-items: center;
    background: #111;
    color: #fff;
    font-family: system-ui, sans-serif;
  }

  empty{ /* preserve */ }

  rpt(@arr.levels[], "div ") {
    background: @arr.colors[@arr.levels[]];
    padding: 12px;
    border-radius: 10px;
    margin: 6px;
  }
</style>
<div>
  <div>
    <div>
      <div>Nested</div>
    </div>
  </div>
</div>
```

## Check

- Four nested frames, each a different color.

## Stretch

- Map labels with an extra array.
- Compare with manual `div div div` CSS—note the DRY win.

## Next

[06 — num & inline](./06-num-inline.md)
