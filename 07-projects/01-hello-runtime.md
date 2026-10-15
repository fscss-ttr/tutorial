# 01 — Hello runtime

## Goal

Run FSCSS in the browser with **no build step** and style a single element.

## Skills

- `runtime.min.js`
- `$variables`
- Basic rule expansion

## Deliverable

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>01 — Hello runtime</title>
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
  <style>
    $bg: #0e0d14;
    $accent: #9d7eff;

    body {
      margin: 0;
      min-height: 100vh;
      display: grid;
      place-items: center;
      background: $bg!;
      font-family: system-ui, sans-serif;
    }

    h1 {
      color: $accent!;
      font-size: 1.5rem;
    }
  </style>
</head>
<body>
  <h1>Hello FSCSS</h1>
</body>
</html>
```

## Check

- Page centers a violet heading on a dark background.
- No CLI required.

## Stretch

- Switch to CLI: put styles in `hello.fscss`, run `fscss hello.fscss hello.css`, link CSS only.
- Try `esm.min.js` + `xfscss.reboot()` instead of runtime ([runtime vs esm](https://fscss.devtem.org/api)).

## Next

[02 — Shorthand card](./02-shorthand-card.md)
