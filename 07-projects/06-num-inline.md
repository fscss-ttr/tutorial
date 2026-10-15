# 06 — num & inline

## Goal

Emit **custom properties from an array** using `count`, `num`, and `inline()`.

## Skills

- `num(<expr>)` with `<>` grouping
- `inline("empty-@arr…")` fragments
- [num / count](https://fscss.devtem.org/num) · [Loops — inline](https://fscss.devtem.org/loop)

## Deliverable

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  @arr values[30, 55, 80, 45, 90]
  @arr idx[count(@arr.values!.length, 1)]

  :root {
    inline("empty{}
      empty-@arr.idx[] {
        $i: @arr.idx[];
        --v$i: num(@arr.values[$i!])%;
      }
    ")
  }

  body {
    margin: 0;
    min-height: 100vh;
    display: flex;
    align-items: flex-end;
    justify-content: center;
    gap: 10px;
    padding: 40px;
    background: #0e0d14;
  }

  .bar {
    width: 36px;
    border-radius: 8px 8px 0 0;
    background: #9d7eff;
  }

  .bar:nth-child(1) { height: var(--v1); }
  .bar:nth-child(2) { height: var(--v2); }
  .bar:nth-child(3) { height: var(--v3); }
  .bar:nth-child(4) { height: var(--v4); }
  .bar:nth-child(5) { height: var(--v5); }
</style>
<div class="bar"></div>
<div class="bar"></div>
<div class="bar"></div>
<div class="bar"></div>
<div class="bar"></div>
```

## Check

- Five bars of different heights from `--v1…--v5`.

## Stretch

- Invert with `num(100 - @arr.values[$i!])%` (chart-style Y).
- Auto-generate `.bar:nth-child` with an index loop.

## Next

[07 — str & theme](./07-str-theme.md)
