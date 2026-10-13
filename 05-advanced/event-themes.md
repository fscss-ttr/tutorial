# Event Themes

The cleanest use of `@event` is a whole theme. One mode parameter, and every color/feeling on the page falls out of it. Combined with token roots, one design can be light, dark, or three flavors deep without a single duplicated rule.

## A Theme Event

```fscss
@event tint(mode) {
  if mode: dark {
    return: #0f172a;
  }
  if mode: light {
    return: #ffffff;
  }
  el {
    return: #f1f5f9;
  }
}

body {
  background: @event.tint(dark);
}
```

One event, many call sites:

```fscss
$text-dark: #f8fafc;
$text-light: #0f172a;

@event ink(mode) {
  if mode: dark { return: $text-dark!; }
  el { return: $text-light!; }
}
```

## The Pairing Pattern

Dark themes need *paired* values — background and foreground that move together. Events keep the pairing in one place:

```fscss
@event pair(mode) {
  if mode: dark {
    return: "#0f172a #f8fafc";   /* bg fg */
  }
  el {
    return: "#ffffff #0f172a";
  }
}

.card {
  background: nth(@event.pair(dark), 1);
  color: nth(@event.pair(dark), 2);
}
```

## Publishing to Token Roots

Where do the results live? The `*-root` pattern (full treatment in [Token Roots](./token-roots.md)) stamps them onto `:root` once:

```fscss
@event themeColor(name, mode) {
  el-if name: primary and mode: dark { return: #3b82f6; }
  el-if name: primary { return: #2563eb; }
  el-if name: surface  { return: #f8fafc; }
  el { return: #0f172a; }
}

:root[data-theme="dark"] {
  --bg:       @event.themeColor(surface, dark);
  --accent:   @event.themeColor(primary, dark);
  --text:     @event.themeColor(ink, dark);
}
```

Hold that thought — the next two lessons are entirely about tokens, roots, and switching.

## Events as a Switchboard

An app with a mode enum becomes a set of events:

```fscss
@event modeSurface(mode) { ... }
@event modeBorder(mode)  { ... }
@event modeText(mode)    { ... }
```

Each is tiny; together they ARE the theme. Components read them per property, quietly.

## Guidelines

1. **One parameter, one concept.** Don't let a theme event also answer layout questions.
2. **Default through `el`.** Unknown modes should resolve to a sane theme, not nothing.
3. **Pair what's paired.** Keep bg/fg couples adjacent in one event or one string.
4. **Compile early.** Look at the compiled CSS regularly; theme rules that vanish are rules that never ran.

## Summary

- Use `@event` to branch an entire theme off a mode parameter.
- Pair backgrounds with foregrounds so states can't drift apart.
- Feed event results into `:root` token slots for reuse everywhere.
- Keep theme events tiny and on one concept.

---

**Next: [Advanced Arrays](./arrays-advanced.md)**