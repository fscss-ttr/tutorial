# Import Aliasing

When two modules export the same name — two themes, two button kits — you have a collision. FSCSS solves it with aliases, and it also lets you bundle several imports into one statement.

## Renaming a Module with `as`

```fscss
@import((theme as light-theme) from vendor-themes)
@import((theme as dark-theme) from vendor-themes)
```

Now call them unambiguously:

```fscss
.light-ui { @light-theme() }
.dark-ui  { @dark-theme() }
```

Aliasing keeps both, avoids the clash, and reads far better than `theme-variant-2`.

## Multiple Modules in One Statement

Group related imports in a single `from`:

```fscss
@import((
  buttons,
  cards,
  badges as chips
) from ui-kit)
```

Three modules, one import line, one name change (`badges` → `chips`).

## Remote Imports

Pull a module from a URL instead of a registry name:

```fscss
@import((tokens) from "https://cdn.example.com/packages/tokens.fscss")
```

Remote imports work exactly like local ones — selective, aliased, or wildcard:

```fscss
@import((*) from "https://cdn.example.com/packages/ui.fscss")
```

Use remotes sparingly: every remote import is a runtime or build-time dependency on that URL staying up.

## Importing the Same Module with Different Aliases

Sometimes you want the *same* module under two names — e.g. a base theme plus a scoped copy. Alias both:

```fscss
@import((brand as brand-default) from design-system)
@import((brand as brand-beta) from design-system)

:root { @brand-default() }
.beta .app { @brand-beta() }
```

## Good Habits

- **Alias to the role**, not the source: `theme as light-theme`, not `theme as t1`.
- **Keep aliases short** — you'll repeat the name on every call.
- **Group by module** — one statement per library, not one per component.
- **Prefer named imports** over `(*)` when you only need a couple of names.

## Checklist When Something Breaks

1. Did the module actually export that name? (Check `@define`/`@fun`/`str` labels.)
2. Is the alias unique across all imports?
3. For remotes: is the URL reachable and CORS-friendly?

## Summary

- `(module as alias)` renames an import to avoid clashes.
- Multiple modules group in one `from (…)` block.
- Remote imports use a quoted URL.
- Alias by role, keep it short, prefer selective imports.

---

**Next: [Numbers and Math](./num-and-math.md)**