# Event Basics (`@event`)

`@event` is a parameterized function that returns a *value* depending on conditions. It brings `if/else` into a stylesheet without a build script — the stylesheet asks "what mode am I in?" and gets an answer.

## The Shape

```fscss
@event theme(mode) {
  if mode: dark {
    return: #111111;
  }
  el {
    return: #ffffff;
  }
}

body {
  background: @event.theme(dark);
  color: @event.theme(light);
}
```

```css
body {
  background: #111111;
  color: #ffffff;
}
```

- `@event name(param)` declares the event.
- `if param: value { return: ...; }` matches a value.
- `el` is the else branch.
- `@event.name(arg)` calls it and yields the returned value.

## The Three Keywords

| Keyword | Meaning |
|---------|---------|
| `if` | First branch — matches when its condition holds |
| `el-if` | Additional condition (an "else if") |
| `el` | Fallback when nothing above matched |

You've seen `if` and `el`. Keep `el-if` ready for the next lesson.

## Events Return Values

An event doesn't emit declarations — it *is* a value. That value can fill any value slot:

```fscss
@event surface(mode) {
  if mode: glass {
    return: rgba(255, 255, 255, 0.08);
  }
  el {
    return: rgba(30, 30, 30, 0.85);
  }
}

.panel {
  background: @event.surface(glass);
  border-radius: 12px;
}
```

## Multiple Calls, Different Answers

Call the same event with different arguments in one rule — each resolves independently:

```fscss
@event accent(part) {
  if part: bg {
    return: #2563eb;
  }
  if part: fg {
    return: #ffffff;
  }
  el {
    return: #0f172a;
  }
}

.cta {
  background: @event.accent(bg);
  color: @event.accent(fg);
  border-color: @event.accent(border);
}

/* border has no branch → el wins → #0f172a */
```

## Events Inside Defines

Events compose everywhere values go — including inside defines:

```fscss
@event tone(level) {
  if level: calm { return: #10b981; }
  el { return: #ef4444; }
}

@define status(band) {
  background: @event.tone(@use(band));
  color: white;
  padding: 8px 12px;
  border-radius: 6px;
}

.ok  { @status(calm) }
.err { @status(loud) }
```

## Naming

Good event names read like questions: `theme(mode)`, `spacing(level)`, `device(size)`. Keep each event to one responsibility — one parameter, one family of values.

## Summary

- `@event name(param) { if … el-if … el { return: … } }` defines a conditional value.
- Call with `@event.name(arg)` inside any value position.
- Resolves per call; falls through to `el` when nothing matches.
- Compose events inside defines and other values freely.

---

**Next: [Event Conditions](./event-conditions.md)**