# Dark Mode

Dark mode is a theme with a system signal: `prefers-color-scheme`. The token system you built handles it perfectly — bind the user's preference to token overrides behind a media query, and let a manual override win when the user decides for themselves.

## The Base

Light is your default on `:root`; dark comes in behind the media query:

```fscss
:root {
  --bg: #ffffff;
  --text: #0f172a;
  --surface: #f8fafc;
  --accent: #2563eb;
  --border: #e2e8f0;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg: #0f172a;
    --text: #f8fafc;
    --surface: #1e293b;
    --accent: #60a5fa;
    --border: #334155;
  }
}
```

One media block, all five tokens, every component follows.

## As a Block Define

Keep dark mode next to light with a define:

```fscss
@define color-tokens() {
  `
  :root {
    --bg: #ffffff;
    --text: #0f172a;
    --surface: #f8fafc;
    --accent: #2563eb;
  }

  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #0f172a;
      --text: #f8fafc;
      --surface: #1e293b;
      --accent: #60a5fa;
    }
  }
  `
}

@color-tokens()
```

## Manual Override Wins

A `data-theme` attribute set by the user should beat the system preference. Scope the attribute override *after* the media query, with equal-or-higher specificity:

```fscss
:root[data-theme="light"] {
  --bg: #ffffff;
  --text: #0f172a;
  --accent: #2563eb;
}

:root[data-theme="dark"] {
  --bg: #0f172a;
  --text: #f8fafc;
  --accent: #60a5fa;
}
```

Both blocks sit outside the media query, so an explicit attribute always wins over `prefers-color-scheme`.

## The Three-State Flow

1. **No attribute** → system `prefers-color-scheme` decides.
2. **`[data-theme="dark"]`** → forced dark.
3. **`[data-theme="light"]`** → forced light.

```html
<script>
  const saved = localStorage.getItem('theme');
  if (saved) {
    document.documentElement.dataset.theme = saved;
  }
</script>
```

Respect it before first paint so there's no flash of the wrong theme.

## Transitioning Between Modes

Cross-fade by transitioning the tokens themselves:

```fscss
:root {
  transition: background-color 0.25s ease, color 0.25s ease;
}

body,
.card {
  transition: background-color 0.25s ease, color 0.25s ease;
}
```

Transition the few properties that move; avoid over-transitioning layout geometry.

## Dark-Mode Specific Touches

Some values deserve a token even though they're not "colors":

```fscss
:root {
  --shadow: 0 4px 12px rgba(15, 23, 42, 0.12);
}
@media (prefers-color-scheme: dark) {
  :root {
    --shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
  }
}

.card {
  box-shadow: var(--shadow);
}
```

Shadows, focus rings, and glows all read as tokens, all theme-aware.

## Summary

- Bind dark values to `@media (prefers-color-scheme: dark)` on the token root.
- Explicit `[data-theme]` overrides beat the system — put them outside the media query.
- A small script hydrates the saved choice before paint.
- Tokenize the whole palette (shadows included) so both modes just work.

---

**Next: [Responsive Patterns](./responsive-patterns.md)**