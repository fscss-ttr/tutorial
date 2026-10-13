# 07 — Alert Boxes

Alerts communicate status in four flavours. Build one `@define` that takes an accent and a tint, then feed it the palette so `info`, `success`, `warning`, and `danger` cost a line each — and let `str()` hold the glyphs.

## The Brief

- `info`, `success`, `warning`, and `danger` variants.
- A bold title plus a message line.
- A leading icon per variant.
- A quiet close button that stays out of the way.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  info: #2563eb;
  success: #10b981;
  warning: #d97706;
  danger: #dc2626;
}

@define alert(accent: #2563eb, tint: #eff6ff) {
  border-left: 4px solid @use(accent);
  background: @use(tint);
  color: @fun.color.ink.value;
  padding: 0.85rem 1rem;
  border-radius: 8px;
  font-size: 0.92rem;
  line-height: 1.5;
  display: flex;
  gap: 0.6rem;
  align-items: flex-start;
}

.alert { @alert() }
.alert-info    { @alert(@fun.color.info.value, #eff6ff) }
.alert-success { @alert(@fun.color.success.value, #ecfdf5) }
.alert-warning { @alert(@fun.color.warning.value, #fffbeb) }
.alert-danger  { @alert(@fun.color.danger.value, #fef2f2) }

.alert-title {
  display: block;
  font-weight: 700;
  margin-bottom: 0.15rem;
}

.alert-icon {
  flex: none;
  font-weight: 700;
  color: @fun.color.info.value;
}

.alert-close {
  margin-left: auto;
  border: none;
  background: none;
  cursor: pointer;
  color: @fun.color.ink.value;
  opacity: 0.5;
  font-size: 1rem;
  line-height: 1;
}

.alert-close:hover { opacity: 1; }
```

The glyphs live in block strings so they aren't scattered through the stylesheet:

```fscss
str(info-mark, "content: \"i\";")
str(check-mark, "content: \"\2713\";")
str(warn-mark, "content: \"!\";")

.alert-info .alert-icon { info-mark }
.alert-success .alert-icon {
  check-mark
  color: @fun.color.success.value;
}
.alert-warning .alert-icon {
  warn-mark
  color: @fun.color.warning.value;
}
```

## The HTML

```html
<div class="alert alert-info">
  <span class="alert-icon"></span>
  <div>
    <span class="alert-title">Heads up</span>
    Scheduled maintenance tonight at 22:00.
  </div>
  <button class="alert-close" aria-label="Dismiss">&times;</button>
</div>

<div class="alert alert-success">
  <span class="alert-icon"></span>
  <div>
    <span class="alert-title">Saved</span>
    Your changes are live.
  </div>
</div>

<div class="alert alert-danger">
  <span class="alert-icon"></span>
  <div>
    <span class="alert-title">Payment failed</span>
    We couldn't charge the card on file.
  </div>
</div>
```

## What Happens

- One `@define` writes four variants; the arguments carry only accent and tint.
- `@fun.color.info.value` is read at each call, so recolouring a variant is a token edit.
- `str()` stores the icon glyph once and the bare label injects it as `content`.
- The close button sits at the end via `margin-left: auto` in a flex row, no absolute positioning.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add an `@define dismissible` that appends the close button's spacing.
2. Give each variant its own darker title colour derived from the accent.
3. Add a `.alert--bordered` variant that drops the tint for an outline look.

## Summary

Pass a colour in, get a variant out. The alert family is a define plus four token reads, and the icons never touch a stylesheet rule.

---

**Next: [08 — Modal Dialog](../08-modal-dialog.md)**
