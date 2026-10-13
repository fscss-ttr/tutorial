# Migrating from Sass

If you know Sass, FSCSS feels familiar — variables, mixins, nesting — but the syntax is *its own*. The mapping below is the whole migration guide. Read your Sass, translate each concept, compile, diff.

## The Concept Map

| Sass | FSCSS |
|------|-------|
| `$var` | `$var!` (trailing `!` to evaluate) |
| `@mixin` / `@include` | `@define` / `@name(args)` |
| `@function` returning a value | `@event` (or plain text reuse) |
| `@use '@file'` | `@import(exec(_file.fscss))` |
| `@use 'mix' as *` | `@import((*) from mix)` |
| `@each` / `@for` | `@arr` + `count()` walks |
| `@if / @else` | `@event` `if / el-if / el` |
| `@media` blocks | Block defines (backtick bodies) |
| `%placeholder` / `@extend` | `@fun.name` composition |
| Maps | `@fun` stores or parallel arrays |
| Lists | `@arr` |
| `darken()` / `lighten()` | Pre-computed values (bake the color) |
| `&-parent` nesting | `&` still works |
| Autoprefixer plugin | `-*-property` for the few you keep |
| `calc($a * $b)` | `num($a! * $b)` + unit after `)` |

## Step 1 — Translation in Place

```scss
/* Sass */
$primary: #2563eb;

@mixin card($pad: 1rem) {
  background: white;
  padding: $pad;
  border-radius: 8px;
}

@for $i from 1 through 4 {
  .mt-#{$i} { margin-top: $i * 4px; }
}
```

```fscss
/* FSCSS */
$primary: #2563eb;

@define card(pad: 1rem) {
  background: white;
  padding: @use(pad);
  border-radius: 8px;
}

@arr n[1, 2, 3, 4]
@arr i[count(4, 1)]

.mt-@arr.n[@arr.i[]] {
  margin-top: num(@arr.n[@arr.i[]] * 4)px;
}
```

## Step 2 — Conditionals Become Events

```scss
/* Sass */
@if $theme == dark {
  background: #0f172a;
} @else {
  background: #ffffff;
}
```

```fscss
@event surface(mode) {
  if mode: dark { return: #0f172a; }
  el { return: #ffffff; }
}

body {
  background: @event.surface(dark);
}
```

## Step 3 — Loops Become Array Walks

```scss
/* Sass @each */
$colors: (primary: #2563eb, danger: #dc2626);
@each $name, $color in $colors {
  .text-#{$name} { color: $color; }
}
```

```fscss
@arr names[primary, danger]
@arr vals[#2563eb, #dc2626]
@arr i[count(2, 1)]

.text-@arr.names[@arr.i[]] {
  color: @arr.vals[@arr.i[]];
}
```

## Step 4 — Nesting

FSCSS keeps `&` and selector nesting the way Sass does:

```scss
.card {
  &:hover { transform: translateY(-2px); }
  &__title { font-weight: 600; }
}
```

```fscss
.card {
  &:hover { transform: translateY(-2px); }
  &__title { font-weight: 600; }
}
```

No change needed.

## Step 5 — Media Queries as Defines

Sass's `@include media(...)` becomes a block define:

```fscss
@define tablet() {
  `
  @media (min-width: 768px) {
    .grid { grid-template-columns: repeat(2, 1fr); }
  }
  `
}

@tablet()
```

## Step 6 — The Compile Gate

Translate file by file; compile after each:

```bash
fscss src/styles.fscss dist/styles.css
diff dist/styles.css dist/previous.css
```

You want the visual diff small even when the syntax diff is large. Translate, compile, compare, commit.

## Gotchas That Bite Sass Devs

1. **The `!`** — forgetting it prints literals silently.
2. **`@use(param)`** — mixin params are gone without it.
3. **Parentheses in `num()`** — use `< >`.
4. **1-indexed arrays** — `@arr.a[0]` is an error/empty; index 1 is first.
5. **`darken()`/`lighten()` don't exist** — pick the darker hex yourself and tokenize it.
6. **Units moved outside math** — `num(4 * 7)px`, not `num(4px * 7)`.

## Summary

- Map each Sass feature to its FSCSS equivalent; most are name-for-name.
- Loops, conditionals, and functions become arrays, `@event`, and defines.
- Nesting and `&` carry over unchanged.
- Translate in small slices and gate each slice with a compile + diff.
- Watch the `!`, `@use`, `< >`, and 1-indexed traps.

---

**Next: [Expert](../06-expert/README.md)**