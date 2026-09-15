# Arrays & Loops

Generate repetitive CSS patterns with loops. Write less code, generate more styles.

## @for Loop

Repeat a block of code a specified number of times:

```fscss
@for $i from 1 to 5 {
  .mt-#{$i} {
    margin-top: $i * 0.25rem;
  }
}
```

Generates:

```css
.mt-1 { margin-top: 0.25rem; }
.mt-2 { margin-top: 0.5rem; }
.mt-3 { margin-top: 0.75rem; }
.mt-4 { margin-top: 1rem; }
```

## Loop Syntax

### Basic Loop

```fscss
@for $i from 1 to 10 {
  .p-#{$i} {
    padding: $i * 0.25rem;
  }
}
```

### Including the End Value

Use `through` instead of `to`:

```fscss
@for $i from 1 through 5 {
  .w-#{$i} {
    width: $i * 10%;
  }
}
```

Generates `.w-1` through `.w-5` (inclusive).

## Practical Examples

### Spacing Utilities

```fscss
/* Margin */
@for $i from 0 to 6 {
  .m-#{$i} { margin: $i * 0.25rem; }
  .mt-#{$i} { margin-top: $i * 0.25rem; }
  .mr-#{$i} { margin-right: $i * 0.25rem; }
  .mb-#{$i} { margin-bottom: $i * 0.25rem; }
  .ml-#{$i} { margin-left: $i * 0.25rem; }
  .mx-#{$i} { margin-left: $i * 0.25rem; margin-right: $i * 0.25rem; }
  .my-#{$i} { margin-top: $i * 0.25rem; margin-bottom: $i * 0.25rem; }
}

/* Padding */
@for $i from 0 to 6 {
  .p-#{$i} { padding: $i * 0.25rem; }
  .pt-#{$i} { padding-top: $i * 0.25rem; }
  .pr-#{$i} { padding-right: $i * 0.25rem; }
  .pb-#{$i} { padding-bottom: $i * 0.25rem; }
  .pl-#{$i} { padding-left: $i * 0.25rem; }
  .px-#{$i} { padding-left: $i * 0.25rem; padding-right: $i * 0.25rem; }
  .py-#{$i} { padding-top: $i * 0.25rem; padding-bottom: $i * 0.25rem; }
}
```

### Typography Scale

```fscss
@for $i from 1 to 8 {
  .text-#{$i} {
    font-size: $i * 0.125rem + 0.75rem;
  }
}
```

Generates `.text-1` through `.text-7` with increasing font sizes.

### Grid Columns

```fscss
@for $i from 1 to 13 {
  .col-#{$i} {
    grid-column: span $i;
  }
}
```

### Z-Index Scale

```fscss
@for $i from 1 to 6 {
  .z-#{$i} {
    z-index: $i * 10;
  }
}
```

### Opacity Scale

```fscss
@for $i from 1 to 10 {
  .opacity-#{$i} {
    opacity: $i * 0.1;
  }
}
```

## Nested Loops

```fscss
@for $i from 1 to 4 {
  @for $j from 1 through 4 {
    .grid-#{$i}-#{$j} {
      grid-template-columns: repeat($i, 1fr);
      gap: $j * 0.5rem;
    }
  }
}
```

Generates combinations like `.grid-2-3`, `.grid-4-1`, etc.

## Arrays

Store multiple values and iterate through them:

```fscss
$colors: (primary: #2563eb, secondary: #64748b, success: #22c55e, danger: #dc2626);

@each $name, $color in $colors {
  .bg-#{$name} {
    background: $color;
  }
  .text-#{$name} {
    color: $color;
  }
}
```

Generates:

```css
.bg-primary { background: #2563eb; }
.text-primary { color: #2563eb; }
.bg-secondary { background: #64748b; }
.text-secondary { color: #64748b; }
/* ... and so on */
```

## @each Loop

Iterate through lists or maps:

### List Iteration

```fscss
$sizes: sm, md, lg, xl;

@each $size in $sizes {
  .btn-#{$size} {
    @if $size == sm { padding: 8px 16px; font-size: 14px; }
    @if $size == md { padding: 12px 24px; font-size: 16px; }
    @if $size == lg { padding: 16px 32px; font-size: 18px; }
    @if $size == xl { padding: 20px 40px; font-size: 20px; }
  }
}
```

### Map Iteration

```fscss
$breakpoints: (
  mobile: 480px,
  tablet: 768px,
  desktop: 1024px,
  wide: 1280px
);

@each $name, $width in $breakpoints {
  @media (min-width: $width) {
    .container {
      max-width: $width;
    }
  }
}
```

## Combining Loops with @define

```fscss
@define spacing-unit(name, value) {
  .m-#{$name} { margin: $value; }
  .p-#{$name} { padding: $value; }
}

$spacing: (0: 0, 1: 0.25rem, 2: 0.5rem, 3: 0.75rem, 4: 1rem);

@each $name, $value in $spacing {
  @spacing-unit($name, $value);
}
```

## Tips

1. **Start from 0 or 1** — Decide based on your use case
2. **Use meaningful prefixes** — `mt-` for margin-top, `p-` for padding
3. **Don't over-generate** — Only create what you'll use
4. **Combine with variables** — Make loops configurable

## Exercises

1. Generate a complete spacing system (margin and padding)
2. Create a typography scale with loop
3. Build a responsive grid system using nested loops

---

**Next: [Modular System](./modular-system.md)**
