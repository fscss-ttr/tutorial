# Beginner

Start your FSCSS journey here. This section covers the fundamentals you need to write effective FSCSS code.

## What You'll Learn

| Lesson | Topic | Time |
|--------|-------|------|
| [Variables](./variables.md) | Store and reuse values | 15 min |
| [Shorthand Syntax](./shorthand-syntax.md) | Write CSS faster | 20 min |
| [Style Stores](./style-stores.md) | Create reusable patterns | 20 min |
| [Basic Selectors](./basic-selectors.md) | Target elements | 15 min |
| [First Project](./first-project.md) | Build something real | 30 min |

## Prerequisites

Before starting this section, make sure you have:

- Completed the [Installation](../02-installation/) section
- Basic understanding of HTML
- Basic understanding of CSS (selectors, properties, values)
- A code editor ready

## How to Use This Section

1. **Read each lesson** in order — they build on each other
2. **Try the examples** — Don't just read, write the code
3. **Complete the exercises** — Practice makes perfect
4. **Build the project** — Apply what you learned

## Your First FSCSS File

Create a file called `style.fscss`:

```fscss
/* Variables */
$primary: #2563eb;
$spacing: 1rem;

/* Simple rule */
.button {
  background: $primary;
  padding: $spacing;
  color: white;
}
```

Compile it:

```bash
fscss style.fscss style.css
```

The output:

```css
.button {
  background: #2563eb;
  padding: 1rem;
  color: white;
}
```

Congratulations! You've written your first FSCSS code.

---

**Start: [Variables](./variables.md)**

---

*Written by [Omonire](https://github.com/Omonire)*
