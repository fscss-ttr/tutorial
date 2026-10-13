# Theme Switching

A token-root gives you tokens; switching gives you themes. The classic mechanism is a `data-theme` attribute on the root element, with token overrides scoped to each theme. Add a line of JS to flip the attribute and the whole app re-skins.

## The Structure

```fscss
:root {
  --bg: #ffffff;
  --text: #0f172a;
  --accent: #2563eb;
  --surface: #f8fafc;
}

:root[data-theme="dark"] {
  --bg: #0f172a;
  --text: #f8fafc;
  --accent: #60a5fa;
  --surface: #1e293b;
}
```

Components never change — they read tokens:

```fscss
body {
  background: var(--bg);
  color: var(--text);
}

.card {
  background: var(--surface);
  border-color: var(--accent);
}
```

## Publishing Tokens with a Define

Keep the two `:root` blocks in one define so themes stay together:

```fscss
@define theme-tokens() {
  `
  :root {
    --bg: #ffffff;
    --text: #0f172a;
    --accent: #2563eb;
  }
  :root[data-theme="dark"] {
    --bg: #0f172a;
    --text: #f8fafc;
    --accent: #60a5fa;
  }
  `
}

@theme-tokens()
```

## The Toggle

```html
<body data-theme="dark">
```

```html
<script>
  const btn = document.getElementById('theme-toggle');
  btn.onclick = () => {
    const root = document.documentElement;
    const next = root.dataset.theme === 'dark' ? 'light' : 'dark';
    root.dataset.theme = next;
    localStorage.setItem('theme', next);
  };
  const saved = localStorage.getItem('theme');
  if (saved) document.documentElement.dataset.theme = saved;
</script>
```

Three lines of JS and the stylesheet had zero work to do. All theming stays in FSCSS.

## Three Themes Deep

Tokens, not just two states:

```fscss
:root[data-theme="tangerine"] {
  --accent: #f97316;
  --surface: #fff7ed;
}

:root[data-theme="ocean"] {
  --accent: #0ea5e9;
  --surface: #f0f9ff;
}
```

Users pick from the palette, the attribute changes, everything follows.

## Scoped Overrides per Component

Per-component themes via inherited token overrides:

```fscss
.chat[data-theme="dark"] {
  --bg: #0b1220;
  --surface: #111a2e;
}
```

The same app token names, re-scoped by an attribute on a subtree.

## With `@event` Themes

When the palette is small and branchy, events can compute tokens instead of hand-maintaining `:root` blocks:

```fscss
@event surface(mode) {
  if mode: dark { return: #1e293b; }
  el { return: #f8fafc; }
}

:root[data-theme="dark"] {
  --surface: @event.surface(dark);
}
```

Use event-themes when branch logic helps, and plain token blocks when it doesn't. Both end in the same `--` tokens.

## Checklist

1. Tokens defined on `:root` with defaults.
2. Each theme is a `:root[data-theme="…"]` override block.
3. Components read `var(--token)`, never hardcode theme colors.
4. JS only flips the attribute — it never knows the palette.
5. Persist choice in `localStorage` and honor it on load.

## Summary

- Themes are token override blocks scoped to `[data-theme="…"]`.
- Components are theme-agnostic via `var(--token)`.
- A tiny attribute-flipping script powers the whole system.
- Combine with token roots and `@event` for clean, maintainable theming.

---

**Next: [Dark Mode](./dark-mode.md)**