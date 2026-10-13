# 08 — Modal Dialog

A modal is two surfaces: a fixed overlay and a centred dialog. Build the dialog once with a `@define`, then use a block define to emit keyframes and a reduced-motion escape hatch.

## The Brief

- A fixed overlay that centres the dialog.
- Header, body, and footer regions.
- A close button and a primary action.
- A subtle entrance animation, disabled for reduced motion.

## The FSCSS

```fscss
@fun(color){
  overlay: rgba(15, 23, 42, 0.6);
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  primary: #2563eb;
}

@fun(space){ md: 1.5rem; }

@define modal() {
  background: @fun.color.surface.value;
  border-radius: 14px;
  width: min(480px, 92vw);
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 24px 48px rgba(15, 23, 42, 0.25);
}

.overlay {
  position: fixed;
  inset: 0;
  background: @fun.color.overlay.value;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: @fun.space.md.value;
  z-index: 50;
}

.modal {
  @modal()
  position: relative;
}

.modal-header {
  %2(padding-top, padding-bottom[: 1rem;])
  padding-left: @fun.space.md.value;
  padding-right: @fun.space.md.value;
  border-bottom: 1px solid rgba(15, 23, 42, 0.08);
  font-weight: 700;
  color: @fun.color.ink.value;
}

.modal-body {
  padding: @fun.space.md.value;
  color: @fun.color.muted.value;
  line-height: 1.6;
}

.modal-footer {
  display: flex;
  justify-content: flex-end;
  gap: 0.75rem;
  padding: 0 1.5rem 1.5rem;
}

.modal-close {
  position: absolute;
  top: 0.75rem;
  right: 0.75rem;
  border: none;
  background: none;
  font-size: 1.15rem;
  line-height: 1;
  cursor: pointer;
  color: @fun.color.muted.value;
}

.modal-close:hover { color: @fun.color.ink.value; }

.btn-primary {
  background: @fun.color.primary.value;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  padding: 0.6rem 1.1rem;
  font-weight: 600;
  cursor: pointer;
}

.btn-ghost {
  background: transparent;
  color: @fun.color.ink.value;
  border: 1px solid rgba(15, 23, 42, 0.15);
  border-radius: 8px;
  padding: 0.6rem 1.1rem;
  cursor: pointer;
}
```

Motion is raw CSS, so it goes in a block define — keyframes and a media query a normal define can't produce:

```fscss
@define modal-motion() {
  `
  @keyframes modal-in {
    from { opacity: 0; transform: translateY(10px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .modal {
    animation: modal-in 0.2s ease;
  }

  @media (prefers-reduced-motion: reduce) {
    .modal { animation: none; }
  }
  `
}

@modal-motion()
```

## The HTML

```html
<div class="overlay" role="dialog" aria-modal="true" aria-labelledby="modal-title">
  <div class="modal">
    <button class="modal-close" aria-label="Close">&times;</button>
    <div class="modal-header" id="modal-title">Delete project?</div>
    <div class="modal-body">
      This removes the project and every deploy under it. This cannot be undone.
    </div>
    <div class="modal-footer">
      <button class="btn-ghost">Cancel</button>
      <button class="btn-primary">Delete</button>
    </div>
  </div>
</div>
```

## What Happens

- `.overlay` is a flex centring frame; `.modal` is only a surface, so the two can change independently.
- The motion define emits real CSS through backticks — `@keyframes` and a media query that declarations-only defines can't express.
- `prefers-reduced-motion` is honoured because the block can write the whole `@media` wrapper.
- `@fun.color.overlay.value` keeps the scrim in the palette alongside every other colour.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a `.modal--wide` size and a full-screen variant under 600px.
2. Fade the overlay separately from the dialog's scale-in.
3. Add `@define modal-size(w)` so width is a keyword instead of inline `min()`.

## Summary

The surface is a define; the animation is a block define. Splitting them means the dialog can be reused in a sheet, a drawer, or a popover with the same tokens.

---

**Next: [09 — Dropdown Menu](../09-dropdown-menu.md)**
