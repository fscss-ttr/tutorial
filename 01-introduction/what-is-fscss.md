# What is FSCSS?

FSCSS stands for **Figured Shorthand CSS**. It's a lightweight CSS preprocessor that lets you write cleaner, shorter, and more maintainable stylesheets.

## The Problem with Plain CSS

Writing CSS can get repetitive. You find yourself typing the same property names, the same values, and the same patterns over and over again. Plain CSS doesn't give you variables, functions, or reusable blocks — at least not without writing a lot of duplicate code.

## How FSCSS Solves This

FSCSS introduces **shorthand syntax** that reduces the amount of code you write. Instead of typing full property names and values, you use concise shortcuts that compile to standard CSS.

Here's a quick example:

**Plain CSS:**
```css
.button {
  width: 150px;
  height: 150px;
  min-height: 150px;
  min-width: 150px;
}
```

**FSCSS:**
```fscss

.button {
  %4(width, height, min-height, min-width [: 150px;])
}
```

FSCSS goes much further than just variables.

## Core Features

### 1. Variables

Store values once, use them everywhere:

```fscss
$primary: #2563eb;
$spacing: 1rem;
$radius: 8px;

.button {
  background: $primary;
  padding: $spacing;
  border-radius: $radius;
}
```

### 2. Style Stores (@fun)

Group related properties together and apply them with one line:

```fscss
@fun(card) {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}

.card {
  @fun.card
  border: 1px solid #e2e8f0;
}
```

### 3. Reusable Blocks (@define)

Create parameterized style definitions:

```fscss
@define button(bg, color) {
  background: @use(bg);
  color: @use(color);
  padding: 12px 24px;
  border-radius: 8px;
}

.btn-primary {
  @button(#2563eb, white)
}

.btn-danger {
  @button(#dc2626, white)
}
```

### 4. Wildcard Vendor Prefixing

Automatically add vendor prefixes:

```fscss
.div {
  -*-transform: rotate(45deg);
}
```

Compiles to:
```css
.div {
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  transform: rotate(45deg);
}
```

### 5. Arrays and Loops

Repeat patterns without repeating code:

```fscss
@import((mirror) from fscss:micros)

@arr colors[red, blue, green]
@mirror(colors, ind)

  .mt-@arr.ind[]{
    margin-top: num(@arr.ind[] * 0.25)rem;
    background: @arr.colors[@arr.ind[]];
  }
```

## How FSCSS Works

FSCSS is a JavaScript-based preprocessor. You write your styles in `.fscss` files, then FSCSS compiles them to standard `.css` files that browsers understand.

There are two ways to use FSCSS:

1. **Build time** — Use the CLI to compile `.fscss` files to `.css`
2. **Runtime** — Include FSCSS via CDN and let it process styles in the browser

## The Philosophy Behind FSCSS

FSCSS was built with three principles in mind:

1. **Simplicity** — Shorthand should feel natural, not confusing
2. **Speed** — Write CSS 50% faster with fewer keystrokes
3. **Flexibility** — Work with any framework, any build tool, any workflow

## What FSCSS Is Not

- FSCSS is **not** a CSS framework — it's a preprocessor
- FSCSS is **not** a replacement for CSS — it compiles to CSS
- FSCSS is **not** complex — it's designed to be learned in hours, not weeks

## Summary

FSCSS is a lightweight CSS preprocessor that uses shorthand syntax to make writing CSS faster and cleaner. It provides variables, style stores, reusable blocks, vendor prefixing, and loops — all with a syntax that's easy to learn and powerful to use.

---

**Next: [Why FSCSS?](./why-fscss.md)**
