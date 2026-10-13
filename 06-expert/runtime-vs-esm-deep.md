# runtime.js vs esm.js

The browser surface is two files, and they serve different jobs. `runtime.js` compiles automatically; `esm.js` compiles when *you* say so. Knowing which to load — and why — is the difference between a working prototype and a maintainable integration.

## The Two Entry Points

| | `runtime.js` | `esm.js` |
|---|---|---|
| Behaviour | Auto-scans the page, compiles on load | Exposes the API, never auto-runs |
| Script tag | `runtime.min.js` | `<script type="module">` |
| You call | Nothing | `xfscss.reboot()` |
| Best for | Prototyping, learning, docs demos | Tooling, manual/build-time use |
| Litmus test | "Open file, styles appear" | "Compile when I decide it's ready" |

## runtime.js — Just Works

```html
<link type="text/fscss" href="style.fscss">
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
```

On load FSCSS finds every FSCSS source (`<link type="text/fscss">` or plain `<style>` content), compiles it, and applies the styles. Zero configuration. Perfect for tutorials and quick demos.

The `defer` matters: script runs after the document parses, so the compiler sees everything.

## esm.js — You Drive

```html
<script type="module">
  import xfscss from "https://cdn.jsdelivr.net/npm/fscss@1.2.5/esm.js";
  xfscss.reboot();
</script>
```

`esm.js` never auto-runs — you import `xfscss` and call `reboot()`. Reboot means "re-scan and (re)compile now". Because it never fires on its own, it's safe to load in a larger app where timing is your business.

## When to Call `reboot()` Again

Content added after first compile needs a re-scan:

```js
const style = document.createElement('style');
style.textContent = '$brand: #2563eb; .new { color: $brand!; }';
document.head.appendChild(style);

xfscss.reboot();  // compile the newly added block too
```

Dynamic dashboards, theme toggles that inject styles, and CMS-rendered FSCSS all ride on manual `reboot()`.

## The Botton Line For Production

Neither file should ship in production. Compile once with the CLI, serve plain CSS, and watch both entry points disappear:

```html
<link rel="stylesheet" href="style.css">
```

The runtime stays for prototyping and for genuinely dynamic, compile-on-the-fly scenarios — not for the main event.

## Choosing

| Need | Use |
|------|-----|
| Open a file, see styles | `runtime.js` |
| A build tool controls compilation | `esm.js` + `reboot()` |
| Dynamically injected FSCSS | `esm.js` + `reboot()` after injection |
| Production app | CLI, no runtime |
| `@random` that must not re-roll | CLI (compile-time randomness) |

## Summary

- `runtime.js` auto-compiles on load — set-and-forget.
- `esm.js` exposes `xfscss` and compiles on `reboot()` — you're in control.
- Dynamic styles mean another `reboot()`.
- Production ships compiled CSS and neither entry point.

---

**Next: [CLI Production Output](./cli-production.md)**