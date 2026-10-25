# Array Methods

Arrays would be plain lists without their methods. Append `!` to an array reference to switch into **method access mode**, then call a method by name.

```css
@arr.name        /* direct output: the items */
@arr.name!.length /* method access mode */
```

## The Methods

| Method | Returns | Example |
|--------|---------|---------|
| `.length` | Number of items | `@arr.n!.length` → `3` |
| `.first` | First item | `@arr.n!.first` |
| `.last` | Last item | `@arr.n!.last` |
| `.list` | Comma-separated list of all items | `@arr.n!.list` |
| `.join(sep)` | Items joined by a custom separator | `@arr.n!.join(+)` |
| `.reverse` | Items in reverse order | `@arr.n!.reverse` |
| `.shuffle` | Items in random order | `@arr.n!.shuffle` |
| `.sort` | Alphabetic or numeric sort | `@arr.n!.sort` |
| `.unique` | Remove duplicate items | `@arr.n!.unique` |
| `.indices` | 1-based index list | `@arr.n!.indices` → `1,2,3` |
| `.randint` | One random item | `@arr.n!.randint` |
| `.segment` | Wrap each item in brackets | `@arr.n!.segment` |
| `.sum` | Sum of numeric items | `@arr.num!.sum` |
| `.min` | Smallest numeric value | `@arr.num!.min` |
| `.max` | Largest numeric value | `@arr.num!.max` |
| `.unit(val)` | Append a unit to each item | `@arr.n!.unit(px)` |
| `.prefix(v)` | Prepend a prefix to each item | `@arr.b!.prefix(btn-)` |
| `.surround(b,a)` | Wrap each item with a before/after | `@arr.b!.surround([,])` |

## Inspection

```css
@arr colors[red, green, blue]

.debug-count  { content: "@arr.colors!.length"; }  /* 3 */
.debug-first  { content: "@arr.colors!.first"; }   /* red */
.debug-last   { content: "@arr.colors!.last"; }    /* blue */
.debug-list   { content: "@arr.colors!.list"; }    /* red, green, blue */
```

## Transformations

```css
@arr colors[#3b82f6, #8b5cf6, #06b6d4]

h1 { background: linear-gradient(@arr.colors!.reverse); }
h2 { color: @arr.colors!.join( | ); }
```

`.sort` and `.unique` clean up data:

```css
@arr tags[beta, alpha, beta, gamma]
.sorted  { content: "@arr.tags!.sort"; }    /* alpha, beta, gamma */
.unique  { content: "@arr.tags!.unique"; }  /* beta, alpha, gamma */
```

## Indices and Segment Wrapping

`.indices` gives you the 1-based positions, and `.segment` wraps each item in brackets:

```css
@arr cols[a, b, c]

.content { content: "@arr.cols!.indices"; }  /* 1,2,3 */
.attrs   { content: "@arr.cols!.segment"; }  /* [a], [b], [c] */
```

`.segment` is exactly what you want when building selector lists or attribute groups.

## Numeric Helpers

These only make sense on numeric arrays:

```css
@arr sizes[8, 16, 24, 32]

.total { content: "@arr.sizes!.sum"; }  /* 80 */
.small { content: "@arr.sizes!.min"; }  /* 8 */
.big   { content: "@arr.sizes!.max"; }  /* 32 */
.px    { content: "@arr.sizes!.unit(px)"; } /* 8px, 16px, 24px, 32px */
```

## Prefix and Surround

Generate class names or wrapped tokens:

```css
@arr names[primary, danger, success]

.btn-classes { content: "@arr.names!.prefix(btn-)"; } /* btn-primary, btn-danger, btn-success */
.wrapped     { content: "@arr.names!.surround(:,;)"; } /* :primary;, :danger;, :success; */
```

## Mutation

Add to or remove from an array with indexed operators:

```css
@arr names[a, b]

@arr.names!+[c, d]  /* append: a, b, c, d */
@arr.names!-[1]    /* remove the first item: b, c, d */
```

This is useful for building an array up in steps, though most code declares arrays in one shot.

## Random

`.randint` is a one-item shuffle; `@random(@arr.n)` picks at compile time too (next-next lesson):

```css
arr palette[#4361ee, #f72585, #7209b7]

.card {
  border: 2px groove @arr.palette!.randint;
}
```

## The Chaining Rule

Methods return **strings**, not arrays. So this is invalid:

```css
@arr.colors!.reverse!.first   /* does NOT chain */
```

Do one transformation at a time, or declare an intermediate array.

## Summary

- `@arr.name!.method` unlocks the method set.
- Inspection: `length`, `first`, `last`, `list`, `indices`.
- Transformation: `join`, `reverse`, `sort`, `unique`, `shuffle`, `segment`.
- Numbers: `sum`, `min`, `max`, `unit`.
- Naming: `prefix`, `surround`.
- Mutate with `!+[...]` and `!-[n]`. Methods never chain.

---

**Next: [Auto-Indexing & Loops](./auto-indexing.md)**
