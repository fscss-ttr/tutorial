# Why FSCSS?

Every developer has felt the frustration of writing repetitive CSS. The same property names, the same values, the same patterns — over and over again. FSCSS was created to solve this problem.

## The Pain Points of Plain CSS

### 1. Repetition

You've written `background-color: #2563eb` a hundred times across your project. When you need to change that color, you have to find and replace every instance.

### 2. No Variables

Plain CSS doesn't have variables. Sure, CSS custom properties exist (`--primary-color`), but they're limited to runtime use and don't work in all contexts.

### 3. No Reusable Patterns

Need the same card style in five different places? Copy and paste. Need to change the card style? Update all five places.

### 4. Verbose Syntax

CSS property names are long. `border-radius`, `background-color`, `transition-property` — it adds up.

## How FSCSS Solves These Problems

### 1. Variables for Everything

```fscss
$primary: #2563eb;
$secondary: #64748b;
$success: #22c55e;
$danger: #dc2626;

.button-primary { background: $primary; }
.button-secondary { background: $secondary; }
.button-success { background: $success; }
.button-danger { background: $danger; }
```

Change the color once, and it updates everywhere.

### 2. Style Stores for Reusable Patterns

```fscss
@fun(card) {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  margin-bottom: 1rem;
}

.product-card { @fun.card; }
.blog-card { @fun.card; }
.user-card { @fun.card; }
```

One definition, applied everywhere. Change it once, update everywhere.

### 3. Shorthand for Speed

Instead of typing full property names, FSCSS lets you use shortcuts:

```fscss
/* Wildcard vendor prefixes */
 -*transform: rotate(45deg);

/* Instead of writing */
-webkit-transform: rotate(45deg);
-moz-transform: rotate(45deg);
-ms-transform: rotate(45deg);
-o-transform: rotate(45deg);
transform: rotate(45deg);
```

### 4. Loops for DRY Code

```fscss
@for $i from 1 to 6 {
  .p-#{$i} { padding: $i * 0.25rem; }
  .m-#{$i} { margin: $i * 0.25rem; }
}
```

Generate 12 classes with 4 lines of code.

## The Numbers Don't Lie

| Metric | Plain CSS | FSCSS | Improvement |
|--------|-----------|-------|-------------|
| Lines of code | 100% | ~50% | 50% less |
| Time to write | 100% | ~50% | 50% faster |
| Maintenance effort | High | Low | Significantly easier |
| Consistency | Manual | Automatic | Guaranteed |

## Who Created FSCSS?

FSCSS was created by **Ekuyik Sam (Mr. Figsh)**, a developer from Nigeria. He saw the problems developers faced with repetitive CSS and built FSCSS to make writing styles faster and cleaner.

The project is maintained by the [fscss-ttr](https://github.com/fscss-ttr) organization (FSCSS Transition Team & Remote Authors).

## Why Not Just Use Other Preprocessors?

Other preprocessors like Sass and SCSS are excellent tools. But they come with complexity:

- **Sass** has a steep learning curve with its indentation-based syntax
- **SCSS** has features you might never use, adding cognitive overhead
- **Both** require significant configuration and build setup

FSCSS is different:

- **Simpler syntax** — Learn in hours, not days
- **Lightweight** — No heavy dependencies
- **Flexible** — Use via npm, CDN, or CLI
- **Modern** — Built for today's web development

## Why Not Just Use Plain CSS?

Plain CSS works, but it's limiting:

- **No variables** — Repeat values everywhere
- **No functions** — Copy and paste patterns
- **No loops** — Write repetitive classes manually
- **No shorthand** — Type full property names every time

FSCSS gives you all of this with a syntax that compiles to clean, standard CSS.

## Real-World Benefits

### Faster Development

Write styles in half the time. Spend more time building features, less time typing CSS.

### Easier Maintenance

Variables and style stores mean changes propagate everywhere. No more hunting through files.

### Better Collaboration

Consistent patterns mean your team writes CSS the same way. Code reviews are faster.

### Professional Results

FSCSS compiles to clean, standard CSS. Your users will never know you used a preprocessor.

## Summary

FSCSS exists because writing CSS shouldn't be tedious. It solves the problems of repetition, verbosity, and inconsistency with a simple, powerful syntax. Whether you're building a personal project or a production application, FSCSS helps you write better CSS, faster.

---

**Next: [Why FSCSS Over Others](./fscss-vs-tailwind.md)**
