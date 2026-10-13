# 06 — Form Elements

Forms are where tokens earn their keep: one field style has to cover text, email, select, and textarea. Define it once with `@define`, then let variables and `%N` handle the family.

## The Brief

- A shared `.field` style for text, select, and textarea.
- Labels, help text, and an error state per field.
- A focus ring and placeholder colour from tokens.
- A `field--sm` size built with a multiplexed declaration.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #64748b;
  border: #cbd5e1;
  focus: #2563eb;
  surface: #ffffff;
  bg: #f8fafc;
  danger: #dc2626;
}

@fun(space){ sm: 0.4rem; md: 0.75rem; lg: 1rem; }

$radius: 8px;

@define field-base() {
  font-family: inherit;
  font-size: 0.95rem;
  color: @fun.color.ink.value;
  background: @fun.color.surface.value;
  border: 1px solid @fun.color.border.value;
  border-radius: $radius!;
  padding: @fun.space.md.value @fun.space.lg.value;
  width: 100%;
  box-sizing: border-box;
  transition: border-color 0.15s ease, box-shadow 0.15s ease;
}

.field {
  @field-base()
}

.field:focus {
  outline: none;
  border-color: @fun.color.focus.value;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.15);
}

.field::placeholder { color: @fun.color.muted.value; }

textarea.field {
  %2(min-height, height[: 7rem;])
  resize: vertical;
}

.field--sm {
  font-size: 0.85rem;
  padding: @fun.space.sm.value @fun.space.md.value;
}

.field--error {
  border-color: @fun.color.danger.value;
}

.field--error:focus {
  box-shadow: 0 0 0 3px rgba(220, 38, 38, 0.15);
}

.field-label {
  display: block;
  font-size: 0.85rem;
  font-weight: 600;
  color: @fun.color.ink.value;
  margin-bottom: 0.35rem;
}

.field-help {
  font-size: 0.78rem;
  color: @fun.color.muted.value;
  margin-top: 0.3rem;
}

.field-error-text {
  color: @fun.color.danger.value;
  font-size: 0.78rem;
  margin-top: 0.3rem;
}

.field-check {
  size: 1.1rem;
  accent-color: @fun.color.focus.value;
  vertical-align: middle;
  margin-right: 0.4rem;
}

.form-group { margin-bottom: 1.25rem; }

body {
  background: @fun.color.bg.value;
  font-family: system-ui, sans-serif;
  color: @fun.color.ink.value;
  padding: 2rem;
}
```

## The HTML

```html
<form class="form" action="#" method="post">
  <div class="form-group">
    <label class="field-label" for="email">Email</label>
    <input class="field" id="email" type="email" placeholder="you@example.com" />
    <div class="field-help">We only email when something ships.</div>
  </div>

  <div class="form-group">
    <label class="field-label" for="topic">Topic</label>
    <select class="field" id="topic">
      <option>General</option>
      <option>Bug report</option>
    </select>
  </div>

  <div class="form-group">
    <label class="field-label" for="msg">Message</label>
    <textarea class="field" id="msg" placeholder="Tell us more…"></textarea>
  </div>

  <div class="form-group">
    <label class="field-label" for="name">Name</label>
    <input class="field field--error" id="name" type="text" />
    <div class="field-error-text">Name is required.</div>
  </div>

  <label class="field-help">
    <input class="field-check" type="checkbox" /> Send me a copy
  </label>
</form>
```

## What Happens

- `field-base` is the contract; select, textarea, and input all inherit it, so only their quirks get extra rules.
- `%2(min-height, height[: 7rem;])` writes two declarations from one multiplexed line.
- `$radius` is build-time and inlined at the single `border-radius` site.
- The error state is a modifier class, so markup stays declarative.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a `field--lg` size and a matching label scale.
2. Turn the focus ring into a reusable `@define focus-ring()` shared by buttons and inputs.
3. Add a floating-label variant that moves the label on `:focus-within`.

## Summary

One define, one radius variable, one multiplexed size line. The whole form family falls out of that, and every new field type is just an override.

---

**Next: [07 — Alert Boxes](../07-alert-boxes.md)**
