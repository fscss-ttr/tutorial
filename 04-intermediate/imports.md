# Imports (`@import`)

A stylesheet grows until it needs splitting. `@import` loads styles from a built-in library, a local path, or a remote URL — and it lets you be picky about what you pull in.

## The Core Shape

```fscss
/* Import one named module */
@import((module) from lib-or-path)

/* Import several named modules */
@import((
  mod-a,
  mod-b
) from lib-or-path)

/* Import everything */
@import((*) from lib-or-path)
```

`lib-or-path` is a library name on the FSCSS registry, a local file, or a URL.

## Selective vs Wildcard

**Selective** — pull exactly what you use:

```fscss
@import((buttons) from ui-kit)
@import((theme-tokens) from ui-kit)
```

**Wildcard** — pull the whole module:

```fscss
@import((*) from ui-kit)
```

The trade-off:

| Import | You get | Use when |
|--------|---------|----------|
| `(buttons)` | Only that exported name | You want a couple of components |
| `(a, b, c)` | Several named exports | You want a specific subset |
| `(*)` | Everything | You want the whole kit, fast |

Unless the module is tiny, prefer named imports: they keep your compiled output small and your dependencies obvious.

## Importing Local Files

Local files use the same syntax. The path points at your source:

```fscss
@import((theme) from ./styles/_theme.fscss)
@import((grid) from ./styles/_grid.fscss)
```

## `exec()` Imports

`exec()` runs the target and merges its output. This is the classic way to split theme and layout files:

```fscss
@import(exec(_theme.fscss))
@import(exec(_layout.fscss))

:root {
  @light-theme()
}

.container {
  @container()
}
```

`exec()` imports the file's compiled content inline, at the point of the import — order matters, exactly like CSS.

## What Exports Look Like

For `@import((buttons) from ui-kit)` to work, `ui-kit` must export something called `buttons`. A module exports its defines and stores:

```fscss
/* ui-kit/_buttons.fscss */
@define buttons() {
  `
  .btn { padding: 0.75rem 1.5rem; border: none; border-radius: 6px; }
  @media (min-width: 768px) {
    .btn { padding: 0.875rem 2rem; }
  }
  `
}
```

Import selectively, then call:

```fscss
@import((buttons) from ui-kit)

@buttons()
```

## A Practical Layout

```fscss
/* main.fscss — the only file you compile */
@import(exec(_theme.fscss))    /* tokens and bases  */
@import(exec(_layout.fscss))   /* container, grid   */
@import(exec(_components.fscss)) /* cards, buttons   */
```

One entry point, clean separation, single compiled output.

## Troubleshooting

- **"Module not found"** — the name isn't exported, or the path is wrong. Check the target's exports.
- **Order bugs** — a define is used before its import ran. Move the import earlier.
- **Circular imports** — module A imports B, B imports A. Restructure; modules shouldn't need each other both ways.

## Summary

- `@import((name) from src)` is selective; `@import((*) from src)` is everything.
- `lib-or-path` is a registry library, a local path, or a URL.
- `exec()` inlines a file's output at the import point.
- Prefer named imports for small bundles and clear dependencies.

---

**Next: [Import Aliasing](./import-aliasing.md)**