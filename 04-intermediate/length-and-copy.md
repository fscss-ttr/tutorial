# Strings: `length()` and `copy()`

Text is data too. FSCSS can measure a string and slice it — which turns labels into widths, avatars into initials, and keeps one source of truth instead of two.

## `length()` — Measure a String

`length("text")` returns the character count, including spaces and punctuation:

```text
length("Hello World") → 11
length("btn")         → 3
```

Typically multiplied and turned into a size:

```fscss
.text-box {
  width: num(length("Hello World") * 10)px;
}
```

```css
.text-box {
  width: 110px;
}
```

A nickname badge scaled to its own text:

```fscss
.nickname {
  font-size: num(length("long-leg") * 4)px;
}
```

## `copy()` — Slice Into a New Variable

`copy(length, variable)` extracts a substring from a value and stores it under a new variable name.

```fscss
body {
  background: #4ff000 copy(4, primary-color);
  color: $primary-color!;
}
```

```css
body {
  background: #4ff000;
  color: var(--primary-color);
}
:root {
  --primary-color: #4ff;
}
```

`copy(4, primary-color)` took the first four characters (`#4ff0` → trimmed to `#4ff`? No) — read it as "copy the first 4 characters of the *previous declaration's value* into `primary-color`". The stored value becomes `--primary-color`.

- A **positive** length counts from the start.
- A **negative** length counts from the end.
- A length longer than the string returns the whole thing.

```fscss
.tag {
  color: #2dd4bf copy(-2, tag-color);   /* last 2 chars */
  content: "$tag-color!";
}
```

## `@ext()` — Slice by Position

`@ext(source, start, length: name)` slices a string by index and stores the result as `@ext.name`:

```fscss
body {
  property: "the red color @ext(4,3: myRed)";
  color: @ext.myRed;
}
```

`@ext(4,3: myRed)` reads: start at character 4, take 3 characters, store as `myRed`. From `the red color`, characters 4–6 are `red`. The literal is displayed with the extracted slice removed, and `@ext.myRed` resolves to the stored value:

```css
body {
  property: "the red color";
  color: red;
}
```

## A Practical Ceremony

Avatar initials from a full name:

```fscss
.avatar {
  content: "@ext(1,2: initials)" " — " "Ada Lovelace";
}
```

Badge trimming from a full label:

```fscss
.badge {
  content: "Label: " "@ext(1,4: short)";
}
```

## Which to Use

| Function | Best for |
|----------|----------|
| `length()` | Deriving a size from how much text there is |
| `copy(n, var)` | Grabbing a fixed-size slice into a variable (first/last n chars) |
| `@ext()`, `@ext(source, start, n: name)` | Positional slices stored as `@ext.name` |

## Summary

- `length("text")` → character count; combine with `num()` for sizes.
- `copy(n, variable)` slices a declaration's value into a `$`-reachable variable.
- `@ext(source, start, length: name)` slices by index into `@ext.name`.
- Keep labels and their extracted values in one declaration so they can't drift.

---

**Next: [Random Values](./random.md)**