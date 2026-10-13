# 27 — Search Bar

A search bar needs to feel tight and responsive without becoming a pile of overrides. Use tokens, a block define for the input group, and an `@event` to gate an inline validation state.

## The Brief

- A `.search` form with an icon slot, input, and a clear/action button.
- Consistent radius, border, and focus ring from tokens.
- A small inline error state driven by an `@event` (valid/invalid).
- Works inside a navbar or a page header at multiple widths.

## The FSCSS

```fscss
@fun(color){
  border: #cbd5e1;
  border-focus: #2563eb;
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  danger: #dc2626;
  ring: rgba(37, 99, 235, 0.15);
}

@fun(space){
  pad: 0.5rem 0.75rem;
  gap: 0.5rem;
}

@define search-root() {
  `
  :root {
    --search-radius: 999px;
    --search-h: 2.75rem;
  }
  `
}

@define search-bar() {
  `
  .search {
    display: inline-flex;
    align-items: center;
    gap: @fun.space.gap.value;
    height: var(--search-h, 2.75rem);
    padding: 0 0.25rem 0 0.75rem;
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.border.value;
    border-radius: var(--search-radius, 999px);
    box-shadow: 0 1px 2px rgba(15, 23, 42, 0.04);
    transition: border-color 0.15s ease, box-shadow 0.15s ease;
  }

  .search:focus-within {
    border-color: @fun.color.border-focus.value;
    box-shadow: 0 0 0 3px @fun.color.ring.value;
  }

  .search-icon {
    color: @fun.color.muted.value;
    font-size: 1rem;
    line-height: 1;
  }

  .search-input {
    height: 100%;
    border: none;
    outline: none;
    background: transparent;
    color: @fun.color.ink.value;
    font-size: 0.98rem;
    padding: 0 0.25rem;
    min-width: clamp(180px, 40vw, 320px);
  }

  .search-input::placeholder {
    color: @fun.color.muted.value;
  }

  .search-btn {
    height: calc(var(--search-h, 2.75rem) - 0.5rem);
    padding: 0 1rem;
    border-radius: calc(var(--search-radius, 999px) - 0.25rem);
    border: none;
    background: @fun.color.border-focus.value;
    color: @fun.color.surface.value;
    font-weight: 600;
    cursor: pointer;
    transition: filter 0.15s ease;
  }

  .search-btn:hover { filter: brightness(1.05); }
  .search-btn:active { filter: brightness(0.98); }
  `
}

@define search-error() {
  `
  .search.is-invalid {
    border-color: @fun.color.danger.value;
    box-shadow: 0 0 0 3px rgba(220, 38, 38, 0.12);
  }

  .search-help {
    margin-top: 0.5rem;
    font-size: 0.85rem;
    color: @fun.color.muted.value;
  }

  .search-help.is-error {
    color: @fun.color.danger.value;
  }
  `
}

@event search-state(valid: 1) {
  if valid == 1 {
    return: "valid";
  }
  el-if valid == 0 {
    return: "invalid";
  }
  el {
    return: "neutral";
  }
}

@search-root()
@search-bar()
@search-error()
```

## The HTML

```html
<form class="search" role="search">
  <span class="search-icon">🔍</span>
  <input class="search-input" type="search" placeholder="Search docs, projects, or people" aria-label="Search">
  <button class="search-btn" type="submit">Search</button>
</form>
<div class="search-help">Press Enter to search</div>
```

## What Happens

- `search-root()` centralizes radius/height as CSS variables so the button can calc() against them without redefining values.
- `:focus-within` on `.search` rings the whole group (icon + input + button) with one tokenized shadow — no extra focus on the naked input alone.
- `@event search-state()` returns a simple string gate (valid/invalid/neutral) for state naming; the `.is-invalid` rule applies the error treatment.
- `clamp()` on `.search-input` keeps it usable in tight nav slots without hard breakpoints creeping in.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Use `@event.search-state(0)` to emit a modifier class name in a small rule set (or document how you'd toggle `.is-invalid`/`.is-valid`).
2. Add a loading state (`.is-loading`) that swaps the icon for a spinner via a block define.
3. Drive `--search-radius` from an array of radius sizes and pick with `@random`/index for a theme variant.

## Summary

Tokens + CSS variables + a tiny `@event` keep the search bar predictable: focus, error, and sizing share the same source of truth and stay composable in headers and sidebars.

---

**Next: [28 — Testimonials](../28-testimonials.md)**