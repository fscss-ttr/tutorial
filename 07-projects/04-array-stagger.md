# 04 — Array stagger

## Goal

Drive **staggered animation delays and colors** from parallel `@arr` lists.

## Skills

- `@arr` · `count(…, 1)` · `[]` auto-loop
- `empty{ /* preserve */ }`
- [Arrays](https://fscss.devtem.org/arrays) · [Loops](https://fscss.devtem.org/loop) · [num](https://fscss.devtem.org/num)

## Deliverable

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  @arr delays[0.1s, 0.3s, 0.5s, 0.7s, 0.9s]
  @arr colors[#3b82f6, #8b5cf6, #ec4899, #f59e0b, #10b981]
  @arr indexes[count(@arr.colors!.length, 1)]

  @keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-18px); }
  }

  body {
    margin: 0;
    min-height: 100vh;
    display: grid;
    place-items: center;
    background: #0b1020;
  }

  .row { display: flex; gap: 12px; }

  .dot {
    width: 48px;
    height: 48px;
    border-radius: 10px;
    animation: bounce 1.4s infinite ease-in-out;
  }

  empty{ /* preserve */ }

  .dot:nth-child(@arr.indexes[]) {
    $i: @arr.indexes[];
    animation-delay: @arr.delays[$i!];
    background: @arr.colors[$i!];
  }
</style>
<div class="row">
  <div class="dot"></div>
  <div class="dot"></div>
  <div class="dot"></div>
  <div class="dot"></div>
  <div class="dot"></div>
</div>
```

## Check

- Five dots bounce out of phase with different colors.

## Stretch

- Replace delay list with `num(@arr.indexes[] * 0.15)s`.
- Read [array methods](https://fscss.devtem.org/array-methods) and log `@arr.colors!.length`.

## Next

[05 — Nested rpt](./05-nested-rpt.md)
