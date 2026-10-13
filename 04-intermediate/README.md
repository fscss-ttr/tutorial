# 04 — Intermediate FSCSS Tutorial

Reuse and structure.

You can write a stylesheet with variables, `%N`, and a couple of `@define` calls. That gets you through small pages. This section is about building things you can reuse across files and repeat without copy-paste: parameterized defines in depth, named stores, arrays and their methods, imports, and the utility functions that do real work at compile time.

## What You'll Learn

| Lesson | Topic | Description |
|--------|-------|-------------|
| [Defining with Parameters](./define-parameters.md) | `@define` | Parameters, defaults, and composition in depth |
| [Block Defines](./block-defines.md) | `@define` | Emit media queries, keyframes, and whole rules |
| [Function Stores](./fun-stores.md) | `@fun` | Named declaration blocks as design tokens |
| [Array Basics](./arrays-basics.md) | `@arr` | Declare and index ordered lists |
| [Array Methods](./array-methods.md) | `@arr` | Transform, join, sort, and mutate arrays |
| [Auto-Indexing & Loops](./auto-indexing.md) | `count()` | Generate rules from a list without a `for` loop |
| [Imports](./imports.md) | `@import` | Selective vs wildcard imports |
| [Import Aliasing](./import-aliasing.md) | `@import` | Aliases, multiple modules, and remote URLs |
| [Numbers and Math](./num-and-math.md) | `num()` | Compile-time arithmetic |
| [Strings: length and copy](./length-and-copy.md) | `length()`, `copy()` | Derive values from text |
| [Random Values](./random.md) | `@random` | Pick a value at compile time |
| [Repeat with rpt()](./rpt-and-content.md) | `rpt()` | Generated content and separators |
| [Debugging with exec()](./exec-and-inline.md) | `exec()`, `inline()` | Compile-time logs and inline blocks |
| [Vendor Prefixing](./vendor-prefixing.md) | `-*-` | Expand a property across vendors |
| [Semantic Patterns](./patterns.md) | `pattern()` | Match on meaning, not names |

## Goal

Build a reusable helper (a define or module) and import it into a page.

---

**Start: [Defining with Parameters](./define-parameters.md)**

---

*Written by [Omonire](https://github.com/Omonire)*
