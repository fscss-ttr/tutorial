# Property Multiplexing with `%N`

Some values belong in more than one place. A perfectly square box has four identical lengths; a spaced card has the same padding on every side. Writing them out is noise. FSCSS lets you assign one value to many properties in a single line using `%N`.

## The Repetition Problem

Plain CSS:

```css
.box {
  width: 50px;
  height: 50px;
  min-width: 50px;
  min-height: 50px;
}
```

Four properties, one idea. Four chances to mistype.

## One Line with `%N`

```fscss
.box {
  %4(width, height, min-width, min-height[: 50px;])
}
```

The number after `%` is a hint of how many properties you are listing — `%2`, `%3`, `%4`, and so on. The value itself goes inside the brackets after a colon, and each declaration is terminated with `;` (except the last, which may omit it).

Compiles to:

```css
.box {
  width: 50px;
  height: 50px;
  min-width: 50px;
  min-height: 50px;
}
```

## Two Properties

`%2` is the most common:

```fscss
.avatar {
  %2(width, height[: 64px;])
  border-radius: 50%;
}
```

```css
.avatar {
  width: 64px;
  height: 64px;
  border-radius: 50%;
}
```

## Three and Beyond

There is no longer a hard cap at `%6` — modern FSCSS accepts larger counts:

```fscss
.panel {
  %3(border-radius, outline-width, min-width[: 5px;])
}
```

```css
.panel {
  border-radius: 5px;
  outline-width: 5px;
  min-width: 5px;
}
```

## `mxs()` and `mx()`

`%N` is the preferred form, but two functions do the same job and give you a little more flexibility.

`mxs()` takes a plain shared value as its last argument:

```fscss
.card {
  mxs(width, height, max-height, max-width, '200px')
}
```

`mx()` expects the colon and semicolon written out explicitly, so it can accept a more complex declaration:

```fscss
.box {
  mx(width, height, ': 200px;')
}
```

Both compile identically to the four-to-six declarations you listed. When in doubt, use `%N` — it reads best.

## Combining with Variables

Multiplexing pairs naturally with variables:

```fscss
$size: 48px;

.icon {
  %2(width, height[: $size;])
  flex-shrink: 0;
}
```

## A Real Example: Uniform Spacing

```fscss
$card-pad: 1.5rem;

.card {
  %2(padding, margin-bottom[: $card-pad;])
  background: white;
  border-radius: 12px;
}
```

## When Not to Use It

If the properties are not really the *same* value, don't force them together. `margin: 8px` and `padding: 12px` are different ideas — splitting them honestly is clearer:

```fscss
.card {
  margin: 8px;
  padding: 12px;
}
```

Multiplexing is for genuine repetition, not for making one line do two unrelated things.

## A Quick Note on Shorthands

From FSCSS 1.2.5 there is an even shorter route for common cases. Instead of `%2(width, height[: 50px;])` you can write:

```fscss
.box {
  size: 50px;
}
```

`size: 50px;` sets both `width` and `height`. `size: 100% 50px;` sets width then height. There is a whole family of these — `w`, `h`, `mx`, `px`, `rounded`, `bg` — and we cover them properly later. For now, know that `%N` always works and is fully explicit.

## Summary

- `%N(prop1, prop2, …[: value;])` assigns one value to many properties.
- `mxs()` takes a plain value; `mx()` wants the explicit `': value;'`.
- Use it only for real repetition.
- Newer FSCSS adds `size`, `w`, `h`, and friends as even shorter sugar.

---

**Next: [Defining Blocks](./define-basics.md)**
