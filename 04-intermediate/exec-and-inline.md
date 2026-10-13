# Debugging with `exec()` (`exec()`, `inline()`)

FSCSS compiles away your stylesheets, so you can't `console.log` the output as easily as JS. Two utilities keep you honest: `exec()` prints to the console while compiling, and `inline()` tames multi-line blocks.

## `exec()` — Compile-Time Console Output

```fscss
exec(_log, 'compiling theme module')
exec(_warn, 'deprecated variable name')
```

- `exec(_log, ...)` prints an informational message.
- `exec(_warn, ...)` prints a warning.
- Neither produces CSS output — they run while FSCSS is compiling.

Pop the file through the CLI and watch the terminal:

```bash
fscss style.fscss style.css
```

```text
compiling theme module
WARN: deprecated variable name
```

## Lighting Up a Stylesheet

Use `exec()` to confirm a branch actually runs — the FSCSS version of a print statement:

```fscss
exec(_log, 'tokens loaded')
exec(_log, 'buttons processed')

@import(exec(_buttons.fscss))

exec(_log, 'final build')
```

You now see the build order in the console, exactly as FSCSS walks the file.

## Confirming Conditional State

`exec()` works alongside values:

```fscss
$env: production;

document { }
```

That won't do it — but a define that reports its argument will:

```fscss
@define report(tag) {
  exec(_log, 'loading' @use(tag))
}

html { @report(theme-dark) }
```

When you're hunting a stale variable, `exec(_warn, 'using old token $brand!')` beats staring at compiled CSS.

## `inline()` — Strip Braces and Format

`inline(\`...\`)` takes a block of CSS, removes curly brackets, and flattens it:

```fscss
inline(`
  :root{
    --primary-color: midnightblue;
    --main-bg: orange;
  }
`)
```

Becomes the inside of a rule:

```text
:root { inline(`
        :root{ ... }`)
```

Output:

```scss
--primary-color: midnightblue;
--main-bg: orange;
```

It's a compact way to embed structured blocks produced elsewhere, for instance inside a define where you want the raw declarations:

```fscss
@define tokens() {
  `
  inline(`
    :root {
      --primary-color: midnightblue;
      --main-bg: orange;
    }
  `)
  `
}
```

## When You Actually Want These

| Tool | Use it when |
|------|-------------|
| `exec(_log, ...)` | Confirming import/define order in a build |
| `exec(_warn, ...)` | Flagging a deprecated value during migration |
| `inline(...)` | Embedding a formatted block into a declaration context |

Neither is for production output — they're your eyes inside the compiler.

## Summary

- `exec(_log, msg)` / `exec(_warn, msg)` print at compile time and emit no CSS.
- `inline(\`...\`)` strips braces and flattens a CSS block.
- Use them to trace build order, catch stale variables, and embed structured tokens.

---

**Next: [Vendor Prefixing](./vendor-prefixing.md)**