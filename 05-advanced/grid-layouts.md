# Grid Layouts

FSCSS 1.2.5+ adds layout shorthand that turns hog-standard grid setup into a couple of characters — and arrays turn "make me a grid system" into data.

## The Shorthands (1.2.5+)

| You write | Compiles to |
|-----------|-------------|
| `cols: 4;` | `display: grid; grid-template-columns: repeat(4, 1fr);` |
| `auto-fit: 240px;` | `repeat(auto-fit, minmax(240px, 1fr))` |
| `stack: 1rem;` | flex column with `gap: 1rem` |
| `hstack: 0.5rem;` | flex row with `gap: 0.5rem` |
| `center: x;` | flex + `justify-content: center` |
| `center: y;` | flex + `align-items: center` |

```fscss
.dashboard {
  cols: 4;
  gap: 1rem;
}
```

```css
.dashboard {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
}
```

## Auto-Fit Responsive Without Media Queries

`auto-fit` hands the "how many columns?" question to the container:

```fscss
.cards {
  display: grid;
  grid-template-columns: auto-fit(260px);
  gap: 1rem;
}
```

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1rem;
}
```

Cards flow from 4-across to 1-across as the width shrinks — zero breakpoints.

## The Generated Column System

Classic spanning utilities from an array:

```fscss
@arr(spans[1, 2, 3, 4, 6, 12])
@arr(i[count(6, 1)])

.col-@arr.spans[@arr.i[]] {
  grid-column: span @arr.spans[@arr.i[]];
}
```

```css
.col-1  { grid-column: span 1;  }
.col-2  { grid-column: span 2;  }
/* … col-3, 4, 6, 12 */
```

Design the list — 1,2,3,4,6,12 — not a mechanical 1..12.

## Grid References as Tokens

Grid templates are values like any other; tokenize them:

```fscss
:root {
  --grid-cols: 1;
  --grid-gap: 1rem;
}

@media (min-width: 768px) {
  :root {
    --grid-cols: 2;
    --grid-gap: 1.5rem;
  }
}

.board {
  display: grid;
  grid-template-columns: repeat(var(--grid-cols), 1fr);
  gap: var(--grid-gap);
}
```

One set of tokens, responsive for free.

## Rows and Areas

Columns via shorthand, rows and areas in plain CSS (longhand always valid):

```fscss
.page {
  cols: 12;
  grid-template-rows: auto 1fr auto;
  grid-template-areas: "head head" "main side" "foot foot";
}

.page header { grid-area: head; }
.page main   { grid-area: main; }
```

Shorthands don't fight longhand — they mix.

## A Practical Card Wall

```fscss
body {
  bg: #f1f5f9;
  ff: system-ui, sans-serif;
}

.wall {
  display: grid;
  grid-template-columns: auto-fit(240px);
  gap: 1rem;
  max-w: 1100px;
  mx: auto;
  px: 1rem;
}
```

Four lines do layout, centering, and spacing — the rest of the stylesheet stays in components.

## Checklist

1. Reach for `cols`/`auto-fit` before writing explicit templates.
2. Responsive grid = tokenized columns + a media block.
3. Generate span systems from designed arrays.
4. Longhand is always fine when shorthand isn't the fit.

## Summary

- `cols: N`, `auto-fit: MIN`, `stack`/`hstack`, `center` shrink grid boilerplate.
- `auto-fit` removes most breakpoints from card walls.
- Arrays generate span/column systems from data.
- Tokenize columns and gaps for responsive-on-autopilot layouts.

---

**Next: [Performance](./performance.md)**