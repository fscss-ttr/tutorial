# 25 — Notification Toast

Toasts are the easiest component to get subtly wrong: they stack, they dismiss, they slide in, and they come in four tones. Build the tones from an event, the stack from a fixed container, and the entrance from a block define.

## The Brief

- A fixed stack in the top-right corner.
- Four tones: info, success, warning, error.
- Enter animation and a close affordance.
- New toasts push older ones down without overlapping.

## The FSCSS

```fscss
@fun(color){
  surface: #ffffff;
  ink: #0f172a;
  info: #2563eb;
  success: #10b981;
  warning: #f59e0b;
  error: #ef4444;
}

@fun(space){
  pad: 0.9rem 1rem;
  gap: 0.6rem;
}

@event tone(kind) {
  if kind: success {
    return: @fun.color.success.value;
  }
  if kind: warning {
    return: @fun.color.warning.value;
  }
  if kind: error {
    return: @fun.color.error.value;
  }
  el {
    return: @fun.color.info.value;
  }
}

@define toast(radius: 12px) {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  background: @fun.color.surface.value;
  color: @fun.color.ink.value;
  border-radius: @use(radius);
  padding: @fun.space.pad.value;
  border-left: 4px solid @event.tone(info);
  box-shadow: 0 12px 28px rgba(15, 23, 42, 0.18);
  animation: toast-in 0.25s ease both;
}
```

The tone comes from the class, so the same `@toast()` body paints itself:

```fscss
.toast { @toast() }

.toast-info    { border-left-color: @event.tone(info); }
.toast-success { border-left-color: @event.tone(success); }
.toast-warning { border-left-color: @event.tone(warning); }
.toast-error   { border-left-color: @event.tone(error); }
```

## The Stack

```fscss
@define stack() {
  `
  .toast-stack {
    position: fixed;
    top: 1rem;
    right: 1rem;
    display: flex;
    flex-direction: column;
    gap: @fun.space.gap.value;
    width: min(360px, calc(100vw - 2rem));
    z-index: 1000;
  }

  @keyframes toast-in {
    from { opacity: 0; transform: translateX(16px); }
    to   { opacity: 1; transform: translateX(0); }
  }

  @media (prefers-reduced-motion: reduce) {
    .toast { animation: none; }
  }
  `
}

@stack()
```

## The HTML

```html
<div class="toast-stack" role="status" aria-live="polite">
  <div class="toast toast-success">
    <span>Deploy finished in 42s.</span>
    <button class="toast-close" aria-label="Dismiss">×</button>
  </div>
  <div class="toast toast-error">
    <span>Build failed on step 3.</span>
    <button class="toast-close" aria-label="Dismiss">×</button>
  </div>
</div>
```

## What Happens

- `@event.tone()` is the single mapping from a tone name to a color, so the four `.toast-*` rules stay one-liners.
- The stack is `position: fixed` with `flex-direction: column`, which is why new toasts naturally push the others down.
- `width: min(360px, calc(100vw - 2rem))` keeps the toast readable on phones without a media query.
- The keyframes live in the same block define as the stack, and the `prefers-reduced-motion` guard disables the animation for anyone who asked.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add an auto-dismiss progress bar with an array of durations.
2. Slide from the bottom on mobile using a second block define.
3. Stack toasts from an array so a burst renders N of them.

## Summary

One event maps tones, one define shapes every toast, and one block define owns the stack and its animation. Small surface, real state.

---

**Next: [26 — Sidebar Navigation](../26-sidebar-navigation.md)**
