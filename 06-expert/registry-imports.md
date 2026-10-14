# FSCSS Registry Imports

How to **`@import` modules** from the FSCSS ecosystem: registry names, GitHub / jsDelivr URLs, local paths, selective lists, and `as` renames.

**Requires:** FSCSS **1.2.x** (folder-style and remote imports: prefer **1.2.1+**; shorthands ecosystem **1.2.5+**).

---

## 1. What a registry import is

```css
@import((export1, export2) from source)
```

| Part | Meaning |
|------|---------|
| `(export1, export2)` | Public `@define` names (or `*` for all) |
| `from source` | Module id, path, or URL |

The compiler **registers** those defines in the current file, then you **call** them (`@export1()`, …).  
No JS bundle is required for the import itself—only the FSCSS runtime or CLI.

---

## 2. Source forms

### 2.1 Registry / package name

Short name resolved by the toolchain (published module or org registry):

```css
@import((*) from overlapui)
@import((*) from st-core@v2)
@import((*) from circle-progress)
@import((*) from patterns)
```

Use when the module is published to npm and/or the FSCSS module index your CLI/runtime already knows.

**Version in the name** (when the package ships parallel APIs):

```css
@import((*) from st-core@v2)
```

### 2.2 Quoted path (local)

```css
@import((*) from "./overlapui.fscss")
@import((my-root, my-stack) from "../lib/my-lib.fscss")
```

Best for monorepos, forks, and unpublished work.

### 2.3 Remote URL (jsDelivr / GitHub)

```css
@import((*) from "https://cdn.jsdelivr.net/gh/fscss-ttr/overlapui.fscss@main/overlapui.fscss")

@import((*) from "https://cdn.jsdelivr.net/gh/fscss-ttr/st-core.fscss@main/st-core@v2.fscss")

@import((*) from "https://cdn.jsdelivr.net/gh/fscss-ttr/st-core.fscss@main/pre-releases/st-core@v2.fscss")
```

**Rules of thumb**

- Quote the URL.
- Pin `@main`, a tag, or a commit for reproducibility.
- Point at the **`.fscss` source**, not only the compiled `.css` (compiled CSS is for `<link>`, not `@import((…) from …)` defines).

### 2.4 npm package file (after `npm install`)

```css
@import((*) from "overlapui/overlapui.fscss")
/* or resolver-specific */
@import((*) from "node_modules/overlapui/overlapui.fscss")
```

Plain CSS consumers skip this and use:

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/overlapui@1.0.0/overlapui.min.css">
```

---

## 3. Export list

### All public defines

```css
@import((*) from overlapui)
```

### Selective (preferred in apps)

```css
@import((
  ou-root,
  avatar-overlap,
  circle-overlap,
  ou-reduced-motion
) from overlapui)
```

Only pull what you call—smaller mental API and less accidental surface.

### Empty / mistake

```css
@import(() from overlapui)   /* no exports — useless */
@import(from overlapui)      /* invalid */
```

---

## 4. Rename with `as`

```css
@import((circle-progress as clp, progress-range as pr) from circle-progress)

@clp(.progress-circle)
.p72 { @pr(72) }
```

| Behavior | Detail |
|----------|--------|
| Scope | **Current file only** |
| Original name | **Unavailable** after rename in that file |
| Other files | Unchanged; they can import the original name |

Use `as` to avoid collisions between modules that share similar define names.

---

## 5. Call order

Typical sequence:

```css
@import((*) from st-core@v2)
@import((ou-root, avatar-overlap) from overlapui)

@st-root()
@ou-root()

@avatar-overlap(.members)
@st-chart-fill(.fill, myData)
```

1. `@import` modules  
2. Root token mixins (`@*-root()`)  
3. Component mixins  
4. Consumer overrides on `:root` or parents  

---

## 6. Runtime vs CLI

| Mode | Import support |
|------|----------------|
| **Browser runtime** | Names + URLs + relative paths (network must allow CDN/GitHub) |
| **CLI `fscss in out`** | Local paths always; registry names if configured; URLs depending on toolchain network |

Production recommendation: **CLI-compile** a single CSS file so production does not depend on runtime `@import` fetches.

```bash
npm install -g fscss@1.2.5
fscss app.fscss app.css
```

---

## 7. Registry-oriented modules (examples)

| Import id | Repository | Notes |
|-----------|------------|--------|
| `st-core@v2` | [st-core.fscss](https://github.com/fscss-ttr/st-core.fscss) | Charts / spider; array-driven |
| `overlapui` | [overlapui.fscss](https://github.com/fscss-ttr/overlapui.fscss) | Also `overlapui.css` on npm |
| `circle-progress` | [Circle-progress.fscss](https://github.com/Figsh/Circle-progress.fscss) | Rings / pie / steps |
| `icon-mask` | [icon-mask.fscss](https://github.com/fscss-ttr/icon-mask.fscss) | Mask icon helpers |
| `patterns` | [patterns.fscss](https://github.com/fscss-ttr/patterns.fscss) | `pattern()` libraries |
| `deepspace` | [deepspace.fscss](https://github.com/fscss-ttr/deepspace.fscss) | Contained scene |
| `camelCase` / `snake_case` | FSCSS module packs | Property aliases via `str()` |

Hub index: [hub.devtem.org](https://hub.devtem.org) · [libraries](https://fscss.devtem.org/libraries).

---

## 8. `fscss:` and folder-style ids

Some setups resolve **namespace / folder** ids (tooling-dependent), e.g. shared micros:

```css
@import((mirror, stagger) from fscss:micros)
```

Or path under a known org tree. If a name fails to resolve, use an explicit **quoted URL or relative path**.

Quoted remote always works when network is available:

```css
@import((mirror) from "https://cdn.jsdelivr.net/gh/fscss-ttr/FSCSS@main/xf/styles/fscss/micros.fscss")
```

---

## 9. Standalone CSS vs registry import

| Need | Mechanism |
|------|-----------|
| Custom selectors, fewer rules | Registry / path **`@import` of `.fscss`** |
| No FSCSS in the project | **`<link>`** to `*.css` / `*.min.css` on npm or jsDelivr |

Do not expect `@import((ou-root) from …)` to work against a **precompiled CSS-only** file—defines exist only in **`.fscss` source**.

---

## 10. Errors & debugging

| Symptom | Check |
|---------|--------|
| Define undefined | Name not in import list; renamed with `as`; wrong module |
| Network failure (runtime) | CDN URL, CORS, ad blockers; prefer CLI for prod |
| Wrong version behavior | Pin `@v2` / package version / jsDelivr tag |
| Loop / selector garbage | Missing `empty{ /* preserve */ }`; see [Loops](https://fscss.devtem.org/loop) |

```css
exec(_log, "@arr.myData!.length")  /* inspect when debugging array modules */
```

---

## 11. Quick reference

```css
/* Registry name */
@import((*) from overlapui)

/* Selective + versioned */
@import((st-root, st-chart-line) from st-core@v2)

/* Rename */
@import((avatar-overlap as faces) from overlapui)

/* Local */
@import((*) from "./my-lib.fscss")

/* Remote pin */
@import((*) from "https://cdn.jsdelivr.net/gh/org/repo@v1.0.0/file.fscss")
```

```html
<!-- CSS-only module build -->
<link rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/overlapui@1.0.0/overlapui.min.css">
```

---

## 12. Related docs

| Topic | URL |
|-------|-----|
| `@define` / modules | https://fscss.devtem.org/define |
| Import feature | https://fscss.devtem.org/import |
| Libraries | https://fscss.devtem.org/libraries |
| Module design | *Module Design* (repo guide) |
| Building a library | *Building an FSCSS Component Library* |

---

**Summary:** Registry imports load **named `@define` exports** from a package id, file path, or URL. Prefer **selective lists**, pin **versions** for CI, use **`as` for collisions**, and **compile to CSS** for production so pages do not depend on live registry fetches.

**Next: [runtime.js vs esm.js](./runtime-vs-esm-deep.md)**
