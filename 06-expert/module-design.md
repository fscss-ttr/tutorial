# FSCSS Module Design

Design principles for **FSCSS modules**: how to structure tokens, mixins, imports, and markup so libraries stay composable, themeable, and small.

Companion to *Building an FSCSS Component Library*.  
**Requires:** FSCSS **1.2.3+** (recommend **1.2.5+**).

---

## 1. Goals

| Goal | Design choice |
|------|----------------|
| **Composable** | Small `@define` units; selective `@import` |
| **Themeable** | All visuals via `--prefix-*` tokens |
| **Unscoped by default** | Selector parameters (`sel:.default`) |
| **CSS-native output** | No required JS runtime for interaction |
| **Dual delivery** | Source `.fscss` + optional compiled `.css` |
| **Predictable** | Documented markup contract + a11y |

A module is a **publisher** of mixins and tokens. The consumer **subscribes** via `@import` and invokes `@mixin(...)`.

---

## 2. Layer model

```text
┌─────────────────────────────────────┐
│  Markup (HTML / framework)          │  classes, data-*, aria-*
├─────────────────────────────────────┤
│  Consumer sheet                     │  @import, @root(), overrides
├─────────────────────────────────────┤
│  Module public API                  │  @*-root, @*-component(sel)
├─────────────────────────────────────┤
│  Module internals                   │  arrays, loops, private helpers
├─────────────────────────────────────┤
│  Design tokens (--prefix-*)         │  single source of visual truth
├─────────────────────────────────────┤
│  Compiled CSS                       │  browser
└─────────────────────────────────────┘
```

**Do not** put page layout (grid of the whole app) inside a component module unless the module *is* a layout shell (e.g. phone frame). Prefer **stack / card / chart** units that sit inside the consumer’s layout.

---

## 3. Token design

### 3.1 Prefix

Pick a short, unique prefix and never collide with peers:

| Module | Prefix |
|--------|--------|
| st-core | `--st-*` |
| overlapui | `--ou-*` |
| circle-progress | `--progress-*` |
| your module | `--my-*` / `--xx-*` |

### 3.2 Root mixin

```fscss
@define xx-root(root:root){`
:@use(root) {
  --xx-accent: #9d7eff;
  --xx-surface: #161422;
  --xx-text: #e8e3ff;
  --xx-muted: #6b6488;
  --xx-radius: 12px;
  --xx-pad: 16px;
  --xx-time: 0.35s;
  --xx-ease: cubic-bezier(0.4, 0, 0.2, 1);
}
`}
```

### 3.3 Token categories

| Category | Examples |
|----------|----------|
| Color | accent, surface, text, muted, border, danger |
| Space | pad, gap, size, overlap |
| Radius | radius, radius-lg |
| Motion | time, ease |
| Component-specific | stroke, rmax, fill-opacity |

### 3.4 Override contract

Consumers override **after** `@xx-root()`:

```css
@xx-root()
:root {
  --xx-accent: #22c55e;
}
```

Or scope:

```css
@xx-root(root.theme-dark)
```

Size helpers are optional sugar:

```fscss
@define xx-size(size:48px){
  --xx-size: @use(size);
}
```

---

## 4. Mixin design

### 4.1 One concern per mixin

| Good | Avoid |
|------|--------|
| `@xx-stack(sel)` | `@xx-everything()` only |
| `@xx-chart-line(sel, data)` | Baking page chrome into the line mixin |
| `@xx-reduced-motion()` | Silent global `* { transition: none }` without opt-in |

Provide a **facade** `@xx-lib()` that installs defaults for demos; production should prefer selective imports.

### 4.2 Selector parameter

Always allow retargeting:

```fscss
@define xx-stack(sel:.xx-stack){`
@use(sel) { … }
@use(sel) > * { … }
`}
```

### 4.3 Data parameter

For generated geometry (charts, spiders, staggers), pass the **array name**, not a fixed length:

```fscss
@define xx-plot(sel, p){`
@use(sel) {
  @arr @use(p)-idx[count(@arr.@use(p)!.length, 1)]
  /* use @arr.@use(p)[$i] … */
}
`}
```

**Points vs renderer (st-core model)**

- **Renderer** (`fill` / `line`): structure from array length.  
- **Points**: write `--xx-pN` (or `--xx-sxN` / `--syN`) on a container.  
- Live updates: JS sets those variables only.

### 4.4 Block vs inline defines

| Form | Use when |
|------|----------|
| `@define name(a:1){ prop: @use(a); }` | Few declarations |
| `@define name(){` … `}` | Multi-selector blocks, media, keyframes |

### 4.5 Naming

```text
@xx-root
@xx-<component>          # avatar-overlap, chart-line
@xx-<component>-<part>   # chart-fill, spider-spokes
@xx-<action>             # reduced-motion
```

Avoid names that look like CSS properties (`@color`, `@margin`).

---

## 5. Interaction & motion

- Prefer **`:hover` / `:focus-within` / `:focus-visible`** over JS for open/close.  
- Put **`tabindex="0"`** (or real controls) in the markup contract so keyboard matches hover.  
- Ship **`@xx-reduced-motion()`** that zeroes transforms/transitions under `prefers-reduced-motion`.  
- Keep durations on tokens (`--xx-time`) so themes can slow or kill motion.

---

## 6. Arrays & loops (when to use)

Use generation only when hand-writing rules does not scale:

| Pattern | Tool |
|---------|------|
| N series points | `@arr` + `count` + `inline` / `clip-path` |
| Stagger N children | index array + `nth-child` or class prefix |
| Nested selector depth | `rpt()` |
| Alias maps | `str()` (camelCase / snake_case modules) |

Always:

```fscss
empty{ /* preserve */ }
```

before loop-driven rule batches when required by FSCSS 1.2.x parsing.

See: [Loops](https://fscss.devtem.org/loop) · [num/count](https://fscss.devtem.org/num) · [Array methods](https://fscss.devtem.org/array-methods).

---

## 7. Import & packaging design

### 7.1 Public export list

Treat the set of `@define` names as the **API**. Breaking renames ⇒ major version or `@v2` path.

### 7.2 Import styles consumers use

```css
/* full */
@import((*) from xx-lib)

/* selective */
@import((xx-root, xx-stack) from xx-lib)

/* rename (local alias) */
@import((xx-stack as stack) from xx-lib)
```

Document that **`as` renames for that file only**; the original name is unavailable after aliasing.

### 7.3 Dual artifact

| Artifact | Design intent |
|----------|----------------|
| `xx-lib.fscss` | Maximum control, tree-shaken by what you call |
| `xx-lib.css` | Zero FSCSS toolchain; fixed default class names |

Standalone entry should only expand **documented default selectors**, not every experimental helper.

---

## 8. Markup contract

Design HTML as carefully as mixins:

1. **Minimal structure** — no wrapper soup.  
2. **Stable hooks** — classes or `sel` targets.  
3. **Data attributes** — e.g. `data-alph`, not content scraping.  
4. **CSS variables on the element** — e.g. `style="--n:5"` for spoke count.  
5. **A11y** — visible or SR text; `aria-label` on icon-only controls.  

Example contract table in README:

| Element | Role |
|---------|------|
| `ul.xx-stack` | Root; `tabindex="0"` |
| `li[data-alph]` | Item; letter + color key |

---

## 9. Composition with other modules

```css
@import((*) from st-core@v2)
@import((*) from overlapui)
@import((*) from xx-lib)

@st-root()
@ou-root()
@xx-root()
```

**Rules**

- Each module owns its prefix.  
- Page layout stays in the consumer sheet.  
- Charts + overlap stacks + progress rings combine by **placing markup**, not by merging mixins into one mega-define.

---

## 10. Versioning

| Change | Version |
|--------|---------|
| New optional mixin / token | Minor |
| New default in `@xx-lib()` that adds CSS weight | Minor (note in changelog) |
| Rename mixin, remove token, change markup meaning | Major or `@v2` file |
| Require newer FSCSS (e.g. 1.2.5 shorthands only) | Document `fscss_version`; bump major if old runtimes break |

---

## 11. Anti-patterns

| Avoid | Prefer |
|-------|--------|
| Global element selectors (`div { … }`) in a component lib | Parameterized `sel` |
| Hard-coded `#9d7eff` in every rule | `var(--xx-accent)` |
| One mixin that draws the whole dashboard | Chart + card + stack modules |
| Assuming 8 data points forever | `count(@arr.data!.length)` |
| JS required for hover open | `:focus-within` + tokens |
| Undocumented class names in compiled CSS | README class list for standalone builds |

---

## 12. Design checklist

- [ ] Unique token prefix  
- [ ] `@xx-root` sets all public tokens  
- [ ] Every visual component accepts `sel` (or documents fixed standalone classes)  
- [ ] Selective import works without `@xx-lib()`  
- [ ] Loops guarded with `empty{ /* preserve */ }` where needed  
- [ ] Reduced-motion story  
- [ ] Markup + a11y documented  
- [ ] Min FSCSS version stated  
- [ ] Optional standalone CSS matches documented defaults only  

---

## 13. Reference modules

| Module | Design notes |
|--------|----------------|
| [st-core](https://github.com/fscss-ttr/st-core.fscss) | Tokens + array-driven Cartesian / spider geometry; points vs renderer split |
| [overlapui](https://github.com/fscss-ttr/overlapui.fscss) | Tokenized rest/open stacks; CSS-only interaction; dual `.fscss` / `.css` |
| [circle-progress](https://github.com/Figsh/Circle-progress.fscss) | Progress tokens + conic/mask patterns via defines |
| [icon-mask](https://github.com/fscss-ttr/icon-mask.fscss) | Pack + color/size helpers |

Language refs: [docs](https://fscss.devtem.org/docs) · [loop](https://fscss.devtem.org/loop) · [define](https://fscss.devtem.org/define).

---

## 14. Summary

**Module design** in FSCSS is token-first, mixin-small, selector-parameterized, and markup-honest. Generate CSS with arrays only when the problem is generative (charts, staggers, spokes). Ship a clear public `@define` list, preserve selective imports, and let the browser run ordinary CSS.

**Next: [Registry Imports](./registry-imports.md)**
