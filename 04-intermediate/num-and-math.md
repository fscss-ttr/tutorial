# Numbers and Math (`num()`)

You can leave the calculator out of CSS. `num(expr)` evaluates arithmetic at compile time — great for scales, ratios, and values that would otherwise mean typo-prone mental math.

## Basic Arithmetic

`num()` supports `+`, `-`, `*`, and `/`:

```fscss
.card {
  max-height: num(40 * 4)px;
}
```

```css
.card {
  max-height: 160px;
}
```

Notice the unit: it attaches *after* the closing parenthesis. `num(40 * 4)px` → `160px`.

## Grouping with Angle Brackets

Inside `num()`, use `<` and `>` to group — not parentheses, which FSCSS treats as part of the value:

```fscss
/* Converts to 100% - 2 * 8px */
.double {
  width: num(<100 - 1> / <4 + 50>)%;
}
```

```text
(100 - 1) / (4 + 50) = 99 / 54 ≈ 1.833…
```

So that example yields `1.83333…%`. More practically:

```fscss
.full {
  margin: num(<100% - var(--pad) * 2>);
}
```

Keep nested math inside `< >` pairs so the order of operations stays predictable.

## Working with Units

The unit always comes after `num(...)`. Mixing units inside an expression is on you — the expression is evaluated numerically. If an operand carries a unit, strip it for math and re-attach at the end:

```fscss
$base: 1rem;

.text-lg {
  font-size: num($base! * 1.25);
}
```

When `$base` is `1rem`, parsing the literal `1rem` inside an expression is fragile. Prefer keeping numeric bases without units and appending units at the call site:

```fscss
$scale: 1.25;
$base-size: 16;

h1 { font-size: num($base-size! * $scale! * $scale!)px; }
```

## Combining with `@random`

`num()` plays happily with other value producers:

```fscss
textarea {
  max-height: num(@random([40, 10, 5, 0]) + 50);
}
```

The random pick is resolved, then added. Each compile (or each render in runtime mode) gets a fresh value in the plausible range.

## A Real Scale Builder

```fscss
@arr indexes[count(6, 1)]

.pad-@arr.indexes[] {
  padding: num(@arr.indexes[] * 4)px;
}
```

Generates `.pad-1` through `.pad-6` at 4px, 8px, 12px, … — a spacing system from one formula.

## Traps

```fscss
/* Wrong: parentheses inside the expression */
width: num((100 - 20)%);      /* use < > instead */

/* Wrong: unit inside the expression */
width: num(40px * 4);          /* put the unit outside */

/* Right */
width: num(<100 - 20> * 0.5)%;
height: num(40 * 4)px;
```

## Summary

- `num(expr)` evaluates `+ - * /` at compile time.
- Append the unit after `)`, keep units out of the expression.
- Group with `< >`, never parentheses.
- Combine with `$` variables and `@random` for dynamic-but-reliable values.

---

**Next: [Strings: length and copy](./length-and-copy.md)**