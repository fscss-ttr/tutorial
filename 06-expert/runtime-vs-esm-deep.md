# `runtime.js` vs `esm.js`

FSCSS **1.2.0+** ships two browser builds. Same compiler APIs; different **loading and boot** behavior.

| | **runtime** | **esm** |
|--|-------------|--------|
| **Files** | `runtime.js` / `runtime.min.js` | `esm.js` / `esm.min.js` |
| **Load** | `<script src="…">` | `import … from "…"` |
| **Auto-run** | Yes — scans the page and processes FSCSS | **No** — you call the API |
| **Loader UI** | May handle page loader opacity / reveal | Does **not** touch loaders |
| **Best for** | Drop-in HTML, demos, docs | Apps, tools, bundlers, controlled boot |

**CDN (example 1.2.5):**

```text
https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js
https://cdn.jsdelivr.net/npm/fscss@1.2.5/esm.min.js
```

npm package and **CLI** are unchanged: `fscss input.fscss output.css`.  
All **1.1.x methods remain supported** on both builds.

---

## 1. `runtime.js` — drop-in page boot

Use when the document *is* the app: styles in `<style>`, or `<link type="text/fscss" href="…">`.

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<link type="text/fscss" href="app.fscss">
```

Or:

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" async></script>
<style>
  @import((*) from overlapui)
  @ou-root()
  /* … */
</style>
```

**Behavior**

- Registers and runs processing for page FSCSS.
- Intended for **browser + auto scan** of HTML.
- Prefer **`defer`** or **`async`** so parsing stays non-blocking.
- Do **not** also load `esm.js` on the same page unless you know you need both (normally pick one).

**When to choose runtime**

- Prototypes, CodePen, docs samples  
- Static sites without a bundler  
- “Add one script tag and go”

---

## 2. `esm.js` — import, no auto-run

Use when you want FSCSS as a **library**: compile strings, drive a playground, or boot only after your app is ready.

```html
<script type="module">
  import xfscss from "https://cdn.jsdelivr.net/npm/fscss@1.2.5/esm.min.js";

  // Page does not auto-compile until you opt in:
  xfscss.reboot();
</script>
```

**Behavior**

- **Does not** auto-run page styles on load.
- **Does not** manage a loader element (no assumed `#loader` / opacity reveal).
- Suitable for **FSCSS→CSS tools**, sandboxes, and non-page pipelines.
- You can still process CSS text via the shared API without calling `reboot()` if you only need `process` / `exec`-style entry points (see [API](https://fscss.devtem.org/api)).

**When to choose esm**

- `type="module"` apps  
- Visual builders / playgrounds (e.g. st-core builder)  
- Running compile **without** scanning the whole document  
- Avoiding double-boot or loader flicker controlled by your own UI

---

## 3. Shared API surface

Both builds expose the same family of methods (names as in your version’s API docs), including:

| Area | Role |
|------|------|
| `reboot` | Boot / re-scan page FSCSS (typical after dynamic inject) |
| `process` / `run` | Compile a source string |
| `exec` | Task-style execution (e.g. TEXT content → style) |
| `inline` / `assign` | Supporting helpers |

Exact signatures: **[API reference](https://fscss.devtem.org/api)**.

---

## 4. Side-by-side examples

### A. Classic page (runtime only)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
  <style>
    @import((*) from st-core@v2)
    @st-root()
    @arr d[40, 60, 55, 80]
    @st-chart-fill(.f, d)
    @st-chart-line(.l, d)
    .chart {
      @st-chart-points(d)
      position: relative;
      height: 200px;
      max-width: 420px;
      background: var(--st-surface);
    }
  </style>
</head>
<body>
  <div class="chart"><div class="f"></div><div class="l"></div></div>
</body>
</html>
```

### B. Controlled boot (esm)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <script type="module">
    import xfscss from "https://cdn.jsdelivr.net/npm/fscss@1.2.5/esm.min.js";

    // e.g. after mounting UI or fetching a .fscss string
    await Promise.resolve(); // your init…
    xfscss.reboot();
  </script>
  <style>
    /* same FSCSS as above */
  </style>
</head>
<body>…</body>
</html>
```

### C. Production (neither required)

```bash
fscss app.fscss app.css
```

```html
<link rel="stylesheet" href="app.css">
```

No `runtime` or `esm` on the live page.

---

## 5. Decision guide

```text
Need a script tag and automatic page compile?
  → runtime.min.js (+ defer/async)

Need import, playground, or compile-without-boot?
  → esm.min.js + explicit reboot() / process()

Shipping to production at scale?
  → CLI → plain CSS (no browser FSCSS build)
```

---

## 6. Migration notes (pre-1.2.0)

Older paths such as `/e/xfscss.js`, `/exec.js`, `/exec.min.js` were consolidated:

| Old idea | Use now |
|----------|---------|
| Exec / auto page script | `runtime.js` / `runtime.min.js` |
| Programmatic / module | `esm.js` / `esm.min.js` |

Update CDN samples from `fscss@1.1.x/exec.min.js` to **`@1.2.5/runtime.min.js`** (or esm when appropriate).

---

## 7. Related

| Topic | Link |
|-------|------|
| API | https://fscss.devtem.org/api |
| Install / quick start | https://fscss.devtem.org/docs |
| npm | https://www.npmjs.com/package/fscss |
| Core repo | https://github.com/Figsh/xfscss |

---

**Summary:** **`runtime.js`** auto-boots the page; **`esm.js`** is opt-in via `import` and explicit API calls. Same language and methods; choose based on how you load and when you want compile to run. For production, prefer the **CLI** and ship CSS only.

**Next: [CLI Production Output](./cli-production.md)**
