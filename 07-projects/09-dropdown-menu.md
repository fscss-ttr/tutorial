# 09 — Dropdown Menu

A dropdown is a trigger plus an absolutely positioned panel of actions. Style the panel once with a `@define`, then theme the items, dividers, and section labels on top.

## The Brief

- A `.dropdown` wrapper with a trigger and a menu panel.
- Menu items with hover, an active, and a disabled state.
- A divider and a small section label.
- Panel surface, radius, and shadow from tokens.

## The FSCSS

```fscss
@fun(color){
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  hover: #f1f5f9;
  primary: #2563eb;
}

@define menu-panel() {
  background: @fun.color.surface.value;
  border: 1px solid rgba(15, 23, 42, 0.08);
  border-radius: 10px;
  box-shadow: 0 12px 28px rgba(15, 23, 42, 0.12);
  padding: 0.35rem;
  min-width: 200px;
}

.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-toggle {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  border: 1px solid rgba(15, 23, 42, 0.15);
  background: @fun.color.surface.value;
  color: @fun.color.ink.value;
  border-radius: 8px;
  padding: 0.5rem 0.9rem;
  cursor: pointer;
  font: inherit;
}

.dropdown-toggle:hover { background: @fun.color.hover.value; }

.dropdown-menu {
  @menu-panel()
  position: absolute;
  top: calc(100% + 6px);
  left: 0;
  z-index: 20;
}

.menu-item {
  display: block;
  width: 100%;
  text-align: left;
  padding: 0.55rem 0.75rem;
  border: none;
  background: none;
  color: @fun.color.ink.value;
  border-radius: 6px;
  cursor: pointer;
  font: inherit;
}

.menu-item:hover { background: @fun.color.hover.value; }

.menu-item--active {
  color: @fun.color.primary.value;
  font-weight: 600;
}

.menu-item:disabled {
  color: @fun.color.muted.value;
  cursor: not-allowed;
}

.menu-divider {
  %2(height, margin-top[: 1px;])
  margin-bottom: 0.35rem;
  border: none;
  background: rgba(15, 23, 42, 0.08);
}

.menu-label {
  font-size: 0.72rem;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: @fun.color.muted.value;
  padding: 0.4rem 0.75rem;
}

.chevron {
  size: 0.5rem;
  border: solid @fun.color.muted.value;
  border-width: 0 1.5px 1.5px 0;
  transform: rotate(45deg);
}
```

## The HTML

```html
<div class="dropdown">
  <button class="dropdown-toggle" aria-haspopup="true" aria-expanded="false">
    Account
    <span class="chevron"></span>
  </button>
  <div class="dropdown-menu" role="menu">
    <div class="menu-label">Profile</div>
    <button class="menu-item" role="menuitem">Your account</button>
    <button class="menu-item menu-item--active" role="menuitem">Settings</button>
    <hr class="menu-divider" />
    <button class="menu-item" role="menuitem">Billing</button>
    <button class="menu-item" role="menuitem" disabled>Invite team</button>
  </div>
</div>
```

## What Happens

- `menu-panel` owns surface, radius, and shadow; the panel rule only adds positioning.
- `%2(height, margin-top[: 1px;])` gives the divider its line and its spacing from one line.
- `size: 0.5rem;` sets the chevron's width and height together via the built-in shorthand.
- Disabled styling is a `:disabled` rule, so no extra class is needed in markup.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Flip the panel to the right edge with a `.dropdown--end` modifier.
2. Add a subtitle line inside each item and align it on a grid.
3. Generate per-item hover delays from an array so the list feels staggered.

## Summary

The panel is a define, the items are variants. A dropdown that themes itself from three tokens is the same component whether it holds two links or ten.

---

**Next: [10 — Progress Bar](../10-progress-bar.md)**
