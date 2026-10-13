# 11 — Tabs Component

Tabs are two lists that must agree: the triggers on top and the panels below. Build both from one array so adding a tab is one word, and keep the active state on a single class.

## The Brief

- A horizontal tab bar with an underline on the active tab.
- Panels below it, one visible at a time.
- Trigger and panel classes generated from one array.
- Shared bar and panel rules emitted by a block define.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #64748b;
  line: #e2e8f0;
  accent: #2563eb;
  surface: #ffffff;
}

@fun(space){ pad: 0.7rem 1.1rem; }

@arr(tabs[overview, activity, settings, billing])
@arr(ti[count(@arr.tabs!.length, 1)])

.tab-@arr.tabs[@arr.ti[]] {
  padding: @fun.space.pad.value;
  border: none;
  background: transparent;
  color: @fun.color.muted.value;
  font: inherit;
  font-weight: 600;
  cursor: pointer;
  border-bottom: 2px solid transparent;
  transition: color 0.15s ease, border-color 0.15s ease;
}

.tab-@arr.tabs[@arr.ti[]]:hover {
  color: @fun.color.ink.value;
}

.tab-@arr.tabs[@arr.ti[]].is-active {
  color: var(--tab-accent, #2563eb);
  border-color: var(--tab-accent, #2563eb);
}

.panel-@arr.tabs[@arr.ti[]] {
  display: none;
  padding: 1.25rem 0;
  color: @fun.color.ink.value;
  line-height: 1.6;
}

.panel-@arr.tabs[@arr.ti[]].is-active {
  display: block;
}
```

The bar itself is real CSS, so it lives in a block define — the accent custom property and the panel wrapper together in one place:

```fscss
@define tabs-shell(accent: #2563eb) {
  `
  .tabs {
    --tab-accent: @use(accent);
    display: flex;
    gap: 0.25rem;
    border-bottom: 1px solid @fun.color.line.value;
    background: @fun.color.surface.value;
  }
  .tabs-panels { max-width: 640px; }
  `
}

@tabs-shell(@fun.color.accent.value)
```

## The HTML

```html
<div class="tabs" role="tablist">
  <button class="tab-overview is-active" role="tab">Overview</button>
  <button class="tab-activity" role="tab">Activity</button>
  <button class="tab-settings" role="tab">Settings</button>
  <button class="tab-billing" role="tab">Billing</button>
</div>

<div class="tabs-panels">
  <section class="panel-overview is-active">A short summary of the account.</section>
  <section class="panel-activity">Everything that changed this week.</section>
  <section class="panel-settings">Preferences and defaults.</section>
  <section class="panel-billing">Invoices and the current plan.</section>
</div>
```

## What Happens

- One array feeds both the trigger and the panel class names, so the two lists cannot drift apart.
- The loop walks `@arr.ti` to stamp four triggers and four panels from one pair of templates.
- The accent is a `--tab-accent` custom property set by the shell define; the active underline reads it with a fallback.
- `@tabs-shell` emits the bar and wrapper as full CSS through a backtick block.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add arrow-key roving focus with a small `[role=tablist]` keyboard handler.
2. Stagger each panel in with its index exposed as an `--i` custom property.
3. Turn the panels into a `str()` block and inject the right one per tab.

## Summary

Triggers and panels share one array, the shell lives in a block define, and the accent is a custom property. Adding a tab is one word in the array and one panel in the HTML.

---

**Next: [12 — Accordion](../12-accordion.md)**
