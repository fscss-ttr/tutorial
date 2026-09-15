# CDN Browser

Use FSCSS directly in your browser without any build tools. Perfect for quick testing and learning.

## Runtime Script

The runtime script auto-scans your page and processes FSCSS styles:

```html
<!DOCTYPE html>
<html>
<head>
  <link type="fscss" href="style.fscss">
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.0/runtime.min.js" async></script>
</head>
<body>
  <h1 class="title">Hello FSCSS</h1>
</body>
</html>
```

## How It Works

1. Include the FSCSS runtime script
2. Link your `.fscss` file with `type="fscss"`
3. FSCSS automatically compiles and applies styles

## Writing FSCSS in HTML

You can also write FSCSS directly in a `<style>` tag:

```html
<!DOCTYPE html>
<html>
<head>
  <style type="text/fscss">
    $primary: #2563eb;

    .title {
      color: $primary;
      font-size: 2rem;
      font-weight: 700;
    }

    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 2rem;
    }
  </style>
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.0/runtime.min.js" async></script>
</head>
<body>
  <div class="container">
    <h1 class="title">Hello FSCSS</h1>
  </div>
</body>
</html>
```

## ESM Module

For module-based projects, use the ESM build:

```html
<script type="module">
  import xfscss from "https://cdn.jsdelivr.net/npm/fscss@1.2.0/esm.js";

  // Process FSCSS when ready
  xfscss.reboot();
</script>
```

## Version Options

| Version | URL | Description |
|---------|-----|-------------|
| Latest | `fscss@1.2.0` | Recommended for production |
| Specific | `fscss@1.1.20` | Use for compatibility |

## Advantages of CDN

- **No installation required** — Just add a script tag
- **Instant testing** — Write FSCSS and see results immediately
- **No build tools** — No npm, no webpack, no configuration
- **Learning friendly** — Focus on FSCSS, not setup

## Limitations of CDN

- **No offline use** — Requires internet connection
- **No build optimization** — Not for production apps
- **Browser processing** — Slight performance overhead
- **No CLI features** — Can't use watch mode or plugins

## Example: Complete Page

```html
<!DOCTYPE html>
<html>
<head>
  <style type="text/fscss">
    $primary: #2563eb;
    $gray: #64748b;
    $radius: 8px;

    @fun(card) {
      background: white;
      border-radius: $radius;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      padding: 1.5rem;
      margin-bottom: 1rem;
    }

    body {
      font-family: system-ui, sans-serif;
      background: #f1f5f9;
      color: #334155;
      margin: 0;
      padding: 2rem;
    }

    .card { @fun.card; }

    .btn {
      background: $primary;
      color: white;
      padding: 0.75rem 1.5rem;
      border-radius: $radius;
      border: none;
      cursor: pointer;
    }

    .text-gray { color: $gray; }
  </style>
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.0/runtime.min.js" async></script>
</head>
<body>
  <div class="card">
    <h2>FSCSS is Working!</h2>
    <p class="text-gray">This page uses FSCSS via CDN.</p>
    <button class="btn">Click Me</button>
  </div>
</body>
</html>
```

## Summary

The CDN method is perfect for learning FSCSS, prototyping, and quick tests. For production projects, use the npm installation instead.

---

**Next: [CLI Usage](./cli-usage.md)**
