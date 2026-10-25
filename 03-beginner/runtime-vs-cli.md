# Runtime vs CLI

FSCSS can run in two places: in the browser, compiling as the page loads, or ahead of time on your machine with the CLI. Both are legitimate. They are for different jobs.

## The Browser Runtime

Drop one script tag on the page and FSCSS compiles itself.

### Linking a `.fscss` File

```html
<!DOCTYPE html>
<html>
<head>
  <link type="text/fscss" href="style.fscss">
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.6/runtime.min.js" defer></script>
</head>
<body>
  <h1 class="title">Hello FSCSS</h1>
</body>
</html>
```

The `type="text/fscss"` link is not a normal stylesheet; FSCSS intercepts it, compiles the source, and injects real CSS.

### Writing FSCSS Inline

You can also write FSCSS straight into a normal `<style>` block. In that case do **not** use `type="text/fscss"` — a plain `<style>` is correct:

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.6/runtime.min.js" defer></script>
<style>
  $primary: #2563eb;

  .title {
    color: $primary!;
    font-size: 2rem;
    font-weight: 700;
  }
</style>
```

### How It Works

1. Add the runtime script.
2. Link your `.fscss` file (or write FSCSS in a `<style>` tag).
3. On load, FSCSS scans the page, compiles the FSCSS it finds, and applies the result.

`runtime.js` auto-scans — you don't call anything. There is a second entry point, `esm.js`, which exposes the same API but never auto-runs:

```html
<script type="module">
  import xfscss from "https://cdn.jsdelivr.net/npm/fscss@1.2.6/esm.js";
  xfscss.reboot(); // compile on demand
</script>
```

Use `esm.js` when you want to decide *when* compilation happens, or when another tool needs to drive it.

## The CLI

The CLI compiles `.fscss` to plain `.css` on your machine, ahead of time:

```bash
fscss input.fscss output.css
```

Example:

```bash
fscss src/styles.fscss dist/styles.css
```

The output is ordinary CSS. The page that uses it has no idea FSCSS exists.

### Wiring It Into npm

```json
{
  "scripts": {
    "build:css": "fscss src/styles.fscss dist/styles.css"
  }
}
```

```bash
npm run build:css
```

## Which One Should You Use?

| Situation | Use |
|-----------|-----|
| Trying FSCSS for the first time | Browser runtime |
| A quick single-file demo | Browser runtime |
| A component playground | Browser runtime |
| Any production site | CLI |
| A build pipeline (CI, bundler) | CLI |
| Multiple stylesheets / imports | CLI |

The reason is simple: browser compilation costs a little performance and ships your FSCSS *source* to the user. Compiling ahead of time ships only the finished CSS and removes the runtime entirely.

## The Recommended Path

Prototype with the runtime; ship with the CLI.

```fscss
/* style.fscss — start here, run in the browser */
$brand: #7c3aed;
.hero { bg: $brand!; }
```

When the design settles:

```bash
fscss style.fscss style.css
```

Then serve `style.css` like any other stylesheet and remove the runtime script.

## Summary

- `runtime.js` compiles in the browser and auto-scans; great for learning and prototyping.
- `esm.js` is the manual module build; call `xfscss.reboot()` yourself when needed.
- The CLI (`fscss in.fscss out.css`) compiles ahead of time — use it for production.
- Prototype in the browser, ship with the CLI.

---

**Next: [Your First Project](./first-project.md)**
