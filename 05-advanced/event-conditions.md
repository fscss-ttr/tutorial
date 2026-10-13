# Event Conditions

One branch is a toggle. Three branches is a policy. `@event` grows from simple switches into small decision tables with `el-if`.

## `if` / `el-if` / `el` Chains

```fscss
@event size(type) {
  if type: small {
    return: 12px;
  }
  el-if type: medium {
    return: 16px;
  }
  el-if type: large {
    return: 20px;
  }
  el {
    return: 24px;
  }
}

h1 {
  font-size: @event.size(large);
}
```

```css
h1 {
  font-size: 20px;
}
```

Evaluation is top to bottom: the first branch whose condition matches wins; `el` only runs if nothing matched.

## Order Matters

Put the most specific conditions first. Because evaluation stops at the first match, a broad `el-if` can shadow the specific ones below it:

```fscss
/* Danger: broad branch first never reaches xl */
@event pad(level) {
  el-if level: big { return: 2rem; }
  el-if level: xl   { return: 3rem; }   /* unreachable */
  el { return: 1rem; }
}

/* Right: narrow first, broad last */
@event pad(level) {
  el-if level: xl   { return: 3rem; }
  el-if level: big { return: 2rem; }
  el { return: 1rem; }
}
```

## Combining with `num()`

Branch results can be computed:

```fscss
@event spacing(level) {
  if level == 1 {
    return: num(4 * 2)px;
  }
  el-if level == 2 {
    return: num(8 * 2)px;
  }
  el {
    return: num(16 * 2)px;
  }
}

.card-medium {
  padding: @event.spacing(2);
}
```

```css
.card-medium {
  padding: 16px;
}
```

## Combining with `$` Variables

Events read global variables, so a theme can flow into every branch:

```fscss
$dark-bg: #111111;
$light-bg: #ffffff;

@event themedBackground(mode) {
  if mode: light {
    return: $light-bg!;
  }
  el {
    return: $dark-bg!;
  }
}

body {
  background: @event.themedBackground(light);
}
```

## Real Decision Tables

**Alert colors:**

```fscss
@event alertColor(kind) {
  if kind: success { return: #10b981; }
  el-if kind: warn  { return: #f59e0b; }
  el-if kind: error { return: #ef4444; }
  el { return: #64748b; }
}

.alert-error  { border-color: @event.alertColor(error); }
.alert-ok     { border-color: @event.alertColor(success); }
```

**Presence in a grid:**

```fscss
@event span(breakpoint) {
  if breakpoint: mobile { return: 12; }
  el-if breakpoint: tablet { return: 6; }
  el { return: 4; }
}

.grid-item {
  grid-column: span @event.span(tablet);
}
```

## Best Practices

- Order branches narrow → broad.
- Keep each event on one parameter.
- Use `el` defensively so every call has an answer.
- If two values always change together (e.g. `bg` and `fg` for the same branch), return the pair through two events or one string:

```fscss
@event scheme(mode) {
  if mode: dark { return: "#111111 #ffffff"; }
  el { return: "#ffffff #111111"; }
}

body {
  background: nth(@event.scheme(dark), 1);
  color: nth(@event.scheme(dark), 2);
}
```

## Summary

- Chains go `if` → `el-if` (×n) → `el`.
- First match wins, so order narrow before broad.
- Branches can compute (`num()`) and read `$` variables.
- Decision tables (alerts, grid spans, modes) are an events sweet spot.

---

**Next: [Event Operators](./event-operators.md)**