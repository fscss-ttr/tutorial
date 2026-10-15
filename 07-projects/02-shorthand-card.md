# 02 — Shorthand card

## Goal

Build a card using **1.2.5 property shorthands** (`stack`, `px`, `rounded`, `bg`, …).

## Skills

- Property shorthands → longhands
- [Shorthands reference](https://fscss.devtem.org/shorthands)

## Deliverable

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  body {
    m: 0;
    min-h: 100vh;
    center: both;
    bg: #0b1020;
    color: #e6edf7;
    ff: system-ui, sans-serif;
  }

  .card {
    stack: 12px;
    px: 16px;
    py: 14px;
    rounded: 12px;
    bg: #121a30;
    border: 1px solid #22304f;
    max-w: 320px;
    shadow: 0 8px 24px rgba(0,0,0,.35);
  }

  .card h2 {
    fs: 1.1rem;
    fw: 600;
    m: 0;
  }

  .card p {
    m: 0;
    color: #9fb0d0;
    fs: 0.9rem;
  }
</style>
<div class="card">
  <h2>Shorthand card</h2>
  <p>stack · px · rounded · bg · max-w</p>
</div>
```

## Check

- Card looks padded, rounded, dark surface.
- Compiled CSS shows `display:flex; flex-direction:column; gap:…` etc.

## Stretch

- Add `hstack` toolbar buttons inside the card.
- Use `mx: auto` and `w: 100%` responsibly on small screens.

## Next

[03 — Multiplex & define](./03-multiplex-define.md)
