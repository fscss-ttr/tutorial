# Variables

Variables are the first thing to understand in FSCSS, and the first place plain CSS starts to hurt. In CSS you repeat `#2563eb` across forty rules and regret it the day the brand color changes. FSCSS variables fix that.

## Declaring a Variable

A variable starts with `$`, followed by a name, a colon, and a value:

```fscss
$primary: #2563eb;
$spacing: 1rem;
$radius: 8px;
```

## Using a Variable

To read a variable, write its name and put a `!` after it:

```fscss
$primary: #2563eb;
$radius: 8px;

.button {
  background: $primary!;
  border-radius: $radius!;
  color: white;
}
```

That trailing `!` is not optional. It tells FSCSS "evaluate this here". Without it, `$primary` would be treated as a literal string.

Compiles to:

```css
.button {
  background: #2563eb;
  border-radius: 8px;
  color: white;
}
```

Notice there is no `$primary` and no `--primary` in the output. FSCSS variables are **build-time**: they are inlined and then they are gone.

## Why `!` at All?

Plain text sometimes contains a `$` you don't want expanded — a price, a placeholder, a math sign. Requiring `!` means FSCSS only touches a variable where you explicitly asked it to. It is the difference between "reuse this value" and "print these characters literally".

## Scope

A variable declared at the top of a file is **global** — visible to everything below it:

```fscss
$brand: #7c3aed;

.card { border-color: $brand!; }
.btn  { background: $brand!; }
```

A variable declared **inside a selector** is scoped to that block only:

```fscss
.card {
  $local-bg: #f1f5f9;
  background: $local-bg!;
  padding: 1rem;
}

.sidebar {
  background: $local-bg!; /* undefined here — this fails */
}
```

Use local variables for values that only make sense in one component, and globals for things that define the whole page.

## Naming Conventions

- Use lowercase with hyphens: `$primary-color`, `$border-radius`, `$font-stack`.
- Group related names with a shared prefix: `$space-sm`, `$space-md`, `$space-lg`.
- Name by *role*, not value: `$danger` beats `$red`.

## `$` Variables vs `--` Custom Properties

These look similar and do different jobs. Keep them straight:

| | `$name` (FSCSS) | `--name` (CSS) |
|---|---|---|
| Resolved | At compile time | At runtime in the browser |
| Present in output | No — inlined | Yes |
| Can change per class / state | No | Yes |
| Can be changed by JavaScript | No | Yes |
| Best for | Constants, math bases | Theming, per-component knobs |

A good rule of thumb: if a designer changes it once and recompiles, use `$`. If a user, a theme, or a script changes it while the page is running, use `--`.

## Block Variables with `str()`

`str(label, "css text")` stores a reusable block of declarations under a name. You then inject it by writing the bare label inside a selector:

```fscss
str(cardStyle, "
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  background: white;
")

.product-card {
  cardStyle
  max-width: 300px;
}
```

This is a lightweight alternative to `@define` when you have a fixed pattern with no parameters. When you need arguments, use `@define` — that's the next-next lesson.

## Common Mistakes

```fscss
/* Forgot the `!` — value won't expand */
.button { background: $primary; }

/* Declared after use — variable is not in scope yet */
.button { color: $brand!; }
$brand: #2563eb;
```

Read order matters. Declare at the top, use below.

## Summary

- Declare with `$name: value;`, read with `$name!`.
- Global variables live at the top of the file; local ones live inside a selector.
- `$` variables vanish at compile time; `--` custom properties survive to the browser.
- Reach for `str()` when you want to store a whole block rather than one value.

---

**Next: [Property Multiplexing](./property-multiplexing.md)**
