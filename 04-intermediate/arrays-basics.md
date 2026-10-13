# Array Basics (`@arr`)

Arrays are how FSCSS does data. Declare a list once and index into it, iterate it, or generate rules from it. Arrays are **1-indexed** and **string-native**: every lookup returns a plain string, and methods don't chain.

## Declaring

```fscss
@arr(colors[#3b82f6, #8b5cf6, #06b6d4, #10b981])
@arr(spacing[0.5rem, 1rem, 1.5rem, 2rem])
```

The parentheses are optional; both of these are valid:

```fscss
@arr names[red, green, blue]
@arr names[red, green, blue]
```

Items are comma-separated. Whitespace is trimmed.

## Indexing

Items are read by position, **starting at 1**:

```fscss
@arr(colors[#3b82f6, #8b5cf6, #06b6d4, #10b981])
@arr(spacing[0.5rem, 1rem, 1.5rem, 2rem])

.primary-button {
  background: @arr.colors[1];
  padding: @arr.spacing[2] @arr.spacing[3];
}
```

```css
.primary-button {
  background: #3b82f6;
  padding: 1.5rem 2rem;
}
```

`@arr.colors[1]` is the first item, `@arr.colors[4]` is the fourth. There is no index 0.

## Arrays Hold Any Text

An array item can be a name, a number, a color, or a whole value:

```fscss
@arr(icons[home, user, settings, bell])
@arr(sizes[1rem, 1.25rem, 1.5rem])
@arr(shadows[0 1px 2px rgba(0,0,0,.1), 0 4px 6px rgba(0,0,0,.15)])
```

## Direct Output

Referencing an array without an index outputs all items as a comma-separated string:

```fscss
@arr(fonts[Inter, system-ui, sans-serif])

body {

  /* font-family: @arr.fonts; will compile to font-family: [Inter, system-ui, sans-serif]; so use !.list instead */
font-family: @arr.fonts!.list;
}
```

```css
body {
  font-family: Inter, system-ui, sans-serif;
}
```

That is handy for font stacks and transition lists.

## Auto-Indexing Preview

The empty brackets form `@arr.name[]` walks *every* item. Used in a selector, it generates one rule per item:

```fscss
@arr(delays[0.1s, 0.3s, 0.5s])

.loading-dot:nth-child(@arr.delays[]) {
  animation-delay: @arr.delays[];
}
```

```css
.loading-dot:nth-child(0.1s) {
  animation-delay: 0.1s;
}
/* ... one rule per item ... */
```

That particular example pairs the *values* as indexes, which you rarely want. The useful version pairs a value array with a *counter* array built from `count()`. That is the whole subject of [Auto-Indexing & Loops](./auto-indexing.md).

## Nesting and Limits

- Array items are strings: `@arr.a[1,2,3]` is the single item `1,2,3` if you quote it, so don't nest arrays by accident.
- Nested array declarations are **not** resolved: `@arr.b[@arr.a!.list]` stores the literal text, not the values.
- Method calls return strings, so they don't chain: `@arr.n!.first.length` is not a thing.

## Declaring vs Mutating

A plain declaration creates or replaces an array. Appending and removing use operators we cover in [Array Methods](./array-methods.md):

```fscss
@arr names[a, b]
@arr names!+[c]   /* append */
@arr names!-[1]   /* remove first */
```

## Summary

- `@arr(name[a, b, c])` declares an ordered, 1-indexed list.
- Read items with `@arr.name[n]`; read the whole list with `@arr.name`.
- Items are strings and can be any text.
- Empty brackets and `count()` drive loops — up next.

---

**Next: [Array Methods](./array-methods.md)**
