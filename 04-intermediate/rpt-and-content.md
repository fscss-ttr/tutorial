# Repeat with `rpt()`

`rpt(count, value)` repeats a value a given number of times. It has exactly one serious job — generated content — and it does it beautifully.

## The Syntax

```fscss
rpt(count, value)
```

- `count` — how many times to repeat.
- `value` — the string to repeat.

## In `content`

The most common use is decorative pseudo-element text:

```fscss
.separator::after {
  content: "rpt(10, '— ')";
}
```

```css
.separator::after {
  content: "— — — — — — — — — —";
}
```

A bullet list rendered without markup:

```fscss
.listing::before {
  content: "rpt(3, '• ')";
}
```

## Repeating Complex Values

The value can be more than one character — a word, a symbol, a marker:

```fscss
.fade::after {
  content: "rpt(5, '…')";
}

.spinner::before {
  content: "rpt(4, '/')";
}
```

## Building a Divider

A repeating element makes a clean cross-page divider:

```fscss
.divider {
  text-align: center;
}

.divider::before,
.divider::after {
  content: "rpt(6, '✦ ')";
  color: #cbd5e1;
}

.divider span {
  font-weight: 600;
}
```

## Adding Rhythm to MFa

Generate a dotted leader for prices, specs, or menus:

```fscss
.spec {
  display: flex;
}

.spec::after {
  content: "rpt(30, '. ')";
  color: #94a3b8;
  margin-inline: 0.5rem;
}
```

## A Table of Common Patterns

| Want | Write |
|------|-------|
| Em-dash rule | `content: "rpt(12, '—')"` |
| Dotted leader | `content: "rpt(20, '. ')"` |
| Stars | `content: "rpt(5, '★')"` |
| Bullets | `content: "rpt(4, '• ')"` |
| Ellipsis trail | `content: "rpt(3, '…')"` |

## Caveats

- `rpt()` is for strings inside `content` (and similar string positions), not for layout math. If you need N flex children, that's an array loop.
- The count is fixed at compile time. It can come from a value but it will be baked in.

## Summary

- `rpt(count, value)` repeats a string.
- Its natural home is `content` on pseudo-elements.
- Perfect for dividers, leaders, bullets, and decorative trails.

---

**Next: [Debugging with exec()](./exec-and-inline.md)**