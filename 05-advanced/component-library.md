# Building an FSCSS Component Library

Guide for shipping a reusable **FSCSS module**: tokens, `@define` mixins, selective imports, optional plain CSS, and packaging for npm / GitHub / jsDelivr.

**Audience:** authors of libraries like [st-core](https://github.com/fscss-ttr/st-core.fscss), [overlapui](https://github.com/fscss-ttr/overlapui.fscss), [circle-progress](https://github.com/Figsh/Circle-progress.fscss).  
**Requires:** FSCSS **1.2.3+** (recommend **1.2.5+**).

---

## 1. What a “component library” is in FSCSS

An FSCSS library is **not** a JS runtime UI kit. It is:

| Piece | Role |
|--------|------|
| **`.fscss` source** | `@define` mixins + design tokens (`--prefix-*`) |
| **Import contract** | `@import((names) from module)` |
| **Compiled CSS** (optional) | Full expand for teams that never run FSCSS |
| **Docs + samples** | Markup patterns, tokens table, minimal HTML |

Consumers get **plain CSS** after CLI compile or browser runtime. Prefer **composition** (small mixins, tokens) over one giant stylesheet.

---

## 2. Repo layout

```text
my-lib.fscss/
├── README.md
├── LICENSE                 # MIT recommended
├── package.json
├── my-lib.fscss            # source of truth
├── my-lib.standalone.fscss # optional: expand all defaults → .css
├── my-lib.css              # optional: CI output
├── my-lib.min.css
├── templates/              # optional demos
│   └── demo/
│       ├── index.html
│       └── demo.fscss
├── .github/workflows/
│   └── compile-standalone.yml
└── CONTRIBUTING.md         # optional
```

**Naming**

- Package / import name: short, lowercase (`overlapui`, `st-core@v2`).
- File: `my-lib.fscss` or versioned `my-lib@v2.fscss` when breaking.

---

## 3. Design tokens first

Expose a single root mixin that sets **CSS custom properties**. Everything visual should read from tokens so consumers theme without forking mixins.

```fscss
@define my-root(root:root){`
:@use(root) {
  --my-bg: #0e0d14;
  --my-surface: #161422;
  --my-accent: #9d7eff;
  --my-text: #e8e3ff;
  --my-muted: #6b6488;
  --my-radius: 12px;
  --my-pad: 16px;
  --my-ease: cubic-bezier(0.4, 0, 0.2, 1);
  --my-time: 0.35s;
}
`}
```

**Helpers** for common knobs:

```fscss
@define my-size(size:48px){
  --my-size: @use(size);
}
```

**Rules**

- Prefix all tokens (`--my-*`, `--st-*`, `--ou-*`) to avoid clashes.
- Defaults live in `@my-root()`; overrides on `:root` or a parent after the mixin runs.
- Document every public token in the README table.

---

## 4. Components as `@define` mixins

### Property-level mixin

```fscss
@define my-card(bg: var(--my-surface), pad: var(--my-pad)) {
  background: @use(bg);
  padding: @use(pad);
  border-radius: var(--my-radius);
  color: var(--my-text);
}
```

### Block mixin (structure / selectors) — use backticks

```fscss
@define my-stack(sel:.my-stack){`
@use(sel) {
  display: flex;
  align-items: center;
  gap: var(--my-pad);
}

@use(sel) > * {
  transition: margin var(--my-time) var(--my-ease);
}
`}
```

### Parameterized selector

Always accept a **selector parameter** so consumers are not locked to one class name:

```fscss
@define my-stack(sel:.my-stack){` … @use(sel) … `}

/* consumer */
@my-stack(.team-faces)
```

### One-shot installer (optional)

```fscss
@define my-lib(){`
@my-root()
@my-stack(.my-stack)
@my-card()
`}
```

Document both **selective** and **one-shot** imports.

---

## 5. Arrays, loops, and advanced CSS

Use core language features when the component needs generated rules:

| Need | Tool | Docs |
|------|------|------|
| Data series / indexes | `@arr`, `count()` | [/arrays](https://fscss.devtem.org/arrays), [/num](https://fscss.devtem.org/num) |
| nth-child / nested selectors | `[]`, `rpt()`, `empty{ /* preserve */ }` | [/loop](https://fscss.devtem.org/loop) |
| Fragments inside values | `inline()` | [/loop](https://fscss.devtem.org/loop)#inline-loop |
| Math | `num(<a - b> / c)` | [/num](https://fscss.devtem.org/num) |
| Reusable generators | `@define` + `@use` | [/define](https://fscss.devtem.org/define) |

**Example pattern** (index array + preserve):

```fscss
@define my-stagger(sel:.item-, n:5, step:0.15){`
empty{ /* preserve */ }
@arr __i[count(@use(n), 1)]
@use(sel)@arr.__i[] {
  animation-delay: num(@arr.__i[] * @use(step))s;
}
`}
```

Prefer **passing the data array into the mixin** over hardcoding length, so charts and spiders scale with `@arr` size (st-core v2 model).

---

## 6. Public API surface

List every public `@define` in the README:

```markdown
| Mixin | Purpose |
|-------|---------|
| `@my-root(root)` | Design tokens |
| `@my-stack(sel)` | Stack layout |
| `@my-lib()` | Install defaults |
```

**Internal** helpers can stay undocumented or prefixed (`@my-__colors`).  
**Breaking changes:** bump major / use `@v2` file path (`st-core@v2`).

---

## 7. Consumer usage

### Runtime prototype

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  @import((*) from my-lib)
  @my-lib()
</style>
```

### Selective import

```css
@import((my-root, my-stack) from my-lib)

@my-root()
@my-stack(.faces)
```

### Remote / pinned

```css
@import((*) from "https://cdn.jsdelivr.net/gh/org/my-lib.fscss@main/my-lib.fscss")
```

### CLI production

```bash
npm install -g fscss@1.2.5
fscss app.fscss app.css
```

Ship only `app.css` on the live site.

---

## 8. Optional standalone CSS

For teams that never install FSCSS:

**`my-lib.standalone.fscss`**

```fscss
@import((*) from "./my-lib.fscss")

@my-root()
@my-stack(.my-stack)
/* expand every default selector you support in the CSS build */
```

**Build**

```bash
fscss my-lib.standalone.fscss my-lib.css
```

**package.json**

```json
{
  "name": "my-lib",
  "version": "1.0.0",
  "description": "…",
  "main": "my-lib.fscss",
  "style": "my-lib.css",
  "files": [
    "my-lib.fscss",
    "my-lib.standalone.fscss",
    "my-lib.css",
    "my-lib.min.css",
    "README.md",
    "templates"
  ],
  "scripts": {
    "build:css": "fscss my-lib.standalone.fscss my-lib.css"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/org/my-lib.fscss.git"
  },
  "bugs": {
    "url": "https://github.com/org/my-lib.fscss/issues"
  },
  "homepage": "https://github.com/org/my-lib.fscss#readme",
  "author": { "name": "Your Name", "url": "https://github.com/you" },
  "funding": {
    "type": "github",
    "url": "https://github.com/sponsors/you"
  },
  "keywords": ["fscss", "css", "ui"],
  "license": "MIT"
}
```

**CI** (sketch): on change to `*.fscss`, run `fscss …`, commit `my-lib.css` on `main`.

Document clearly:

| Path | Audience |
|------|----------|
| `my-lib.fscss` | Selective, customizable |
| `my-lib.css` | Drop-in `<link>` |

---

## 9. Markup contract

Document **required HTML** next to each mixin:

- Class names (or custom `sel`)
- Attributes (`data-alph`, `--n` for satellite count)
- Accessibility (`tabindex="0"`, `aria-label`, real text for screen readers)
- Reduced motion: `@my-reduced-motion()` or document `prefers-reduced-motion`

Keep interaction **CSS-first** (`:hover`, `:focus-within`) when possible.

---

## 10. README checklist

- [ ] One-sentence description  
- [ ] FSCSS version requirement  
- [ ] Install: FSCSS import **and** optional CSS/npm  
- [ ] Token table (`--my-*`)  
- [ ] Mixin table  
- [ ] Markup examples  
- [ ] Selective vs `@my-lib()`  
- [ ] Link to [FSCSS docs](https://fscss.devtem.org/docs) / [hub](https://hub.devtem.org)  
- [ ] License  

Mirror structure of mature modules (st-core, overlapui) for consistency across [fscss-ttr](https://github.com/fscss-ttr).

---

## 11. Quality bar

1. **Token-driven** — no hard-coded one-off colors in public mixins when a token exists.  
2. **Selector parameters** — never force a single global class only.  
3. **No leaked selectors** — use `empty{ /* preserve */ }` around array loops.  
4. **Works with CLI and runtime** — test both.  
5. **Version floor** — state min FSCSS (e.g. 1.2.3 for length-driven `count`).  
6. **Samples** — at least one HTML page under `templates/` or `samples/`.  
7. **Prefix** — avoid colliding with `--st-*`, `--ou-*`, etc.

---

## 12. Minimal starter template

```fscss
/* my-lib.fscss — FSCSS component library starter */

@define my-root(root:root){`
:@use(root) {
  --my-accent: #6366f1;
  --my-surface: #12121a;
  --my-text: #f1f5f9;
  --my-radius: 12px;
}
`}

@define my-box(sel:.my-box){`
@use(sel) {
  background: var(--my-surface);
  color: var(--my-text);
  border-radius: var(--my-radius);
  padding: 16px;
  border: 1px solid color-mix(in srgb, var(--my-accent) 25%, transparent);
}
`}

@define my-lib(){`
@my-root()
@my-box(.my-box)
`}
```

**Smoke test**

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
<style>
  @import((*) from "./my-lib.fscss")
  @my-lib()
  body { margin: 0; min-height: 100vh; display: grid; place-items: center; background: #0b0a12; }
</style>
<div class="my-box">Hello library</div>
```

---

## 13. Ecosystem links

| Resource | URL |
|----------|-----|
| FSCSS docs | https://fscss.devtem.org/docs |
| Loops / arrays / num / str | https://fscss.devtem.org/loop · /arrays · /num · /str |
| Hub | https://hub.devtem.org |
| Core | https://github.com/Figsh/xfscss |
| Modules org | https://github.com/fscss-ttr |

---

## 14. Summary

1. **Tokens** in `@*-root`  
2. **Components** as `@define` with `sel` parameters  
3. **Arrays/loops** only when generation is required  
4. **README + samples** as the real API  
5. **Optional** standalone CSS + CI for non-FSCSS users  
6. **npm `style` + `main`** pointing at CSS and source  

Build small, prefix tokens, document markup, and ship CSS the browser already understands.

**Next: [Grid Layouts](./grid-layouts.md)**
