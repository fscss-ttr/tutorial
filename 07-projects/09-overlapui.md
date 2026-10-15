# 09 — overlapui stacks

## Goal

Use the **overlapui** module for avatar + circle menu stacks (CSS-only open on hover/focus).

## Skills

- Selective or full module import
- `--ou-*` tokens
- Markup contract + `tabindex`
- [overlapui](https://github.com/fscss-ttr/overlapui.fscss) · npm `overlapui`

## Deliverable (FSCSS)

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  @import((*) from overlapui)

  @ou-root()
  @avatar-overlap(.avatar-overlap)
  @circle-overlap(.circle-overlap)
  @ou-reduced-motion()

  :root {
    --ou-center-bg: #6366f1;
    --ou-ring: #0f172a;
  }

  body {
    margin: 0;
    min-height: 100vh;
    display: grid;
    place-items: center;
    gap: 48px;
    background: #0b1220;
  }
</style>

<ul class="avatar-overlap" tabindex="0">
  <li data-alph="g">Gina</li>
  <li data-alph="h">Hank</li>
  <li data-alph="i">Ivy</li>
  <li data-alph="j">Jess</li>
</ul>

<div class="circle-overlap" style="--n:5" tabindex="0">
  <button type="button" aria-label="Menu">+</button>
  <a href="#" aria-label="Home">H</a>
  <a href="#" aria-label="Search">S</a>
  <a href="#" aria-label="Chat">C</a>
  <a href="#" aria-label="Groups">G</a>
  <a href="#" aria-label="Profile">P</a>
</div>
```

## Alternative (CSS-only, no FSCSS)

```html
<link rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/overlapui@1.0.0/overlapui.min.css">
```

Same class names; theme with `--ou-*` only.

## Check

- Avatars overlap; focus/hover spreads them.
- Circle fans out five actions; center has `aria-label`.

## Stretch

- Selective import: only `ou-root`, `avatar-overlap`, `ou-reduced-motion`.
- Put avatars in a table “Members” cell.

## Next

[10 — Dashboard compose](./10-dashboard-compose.md)
