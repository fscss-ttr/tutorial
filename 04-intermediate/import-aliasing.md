**FSCSS `@import`** is the modular import system for bringing in `@define` mixins (and whole libraries) from other `.fscss` files or the registry.

### Core mental model

1. **Registration** — A module file uses `@define` to register blueprints (no CSS is emitted yet).
2. **Extraction** — `@import` pulls the named defines into *this* file’s scope.
3. **Substitution** — You call the mixin (`@name(...)`) → it expands to real CSS.

Nothing runs automatically. You must invoke the imported names.

---

### Syntax forms (v1.1.16+ / recommended 1.2.6+)

```css
/* 1. Named imports (preferred) */
@import((flex-x, flex-center) from flex-control)

/* 2. Aliases (rename for this file only) */
@import((circle-progress as cp, progress-range as pr) from circle-progress)

/* 3. Everything */
@import((*) from "animations.fscss")

/* 4. Registry libraries (no quotes) */
@import((*) from st-core@v2)
@import((*) from form)
@import((mirror as m) from fscss:micros)

/* 5. Local path (quotes) */
@import((card, card-body) from "./components/card.fscss")

/* 6. Remote URL (quotes) */
@import((btn, btn-primary) from "https://cdn.example.com/buttons.fscss")
```

After importing you **must call** the mixins:

```css
@form-root()
@cp(.progress-circle)
.p75 { @pr(75) }

.container {
  @flex-center()
}
```

---

### Important rules (especially v1.2.6)

- `as` **fully renames** the binding for the current file. The original name becomes undefined here.
- A file owns the names it imported (including aliases). Downstream files can re-import *those* local names.
- Prefer selective named imports over `*` (smaller output, clearer intent).
- Registry names → **no quotes**. Paths and URLs → **quotes**.
- Older form still works: `@import(exec("file.fscss"))`

---

### Practical examples

**Form module (from previous conversation):**
```css
@import((*) from form)

@form-root()
@form-group(.form-group)
@form-input(.form-input)
@form-label(.form-label)
@form-float(.form-group, .form-input, .form-label)
/* … etc */
```

**Progress circle:**
```css
@import((
  circle-progress as cp,
  progress-range as pr,
  progress-root as root
) from circle-progress)

@root()
@cp(.progress-circle)
.p85 { @pr(85) }
```

**Local + alias chain:**
```css
/* style.fscss */
@import((mirror as m) from fscss:micros)

/* app.fscss */
@import((m as mirr) from "style.fscss")
@mirr(colors, indices)   /* works */
/* @m or @mirror → error in this file */
```

---

### Browser vs production

| Mode       | How imports work                          |
|------------|-------------------------------------------|
| Runtime    | `runtime.min.js` resolves registry + URLs |
| CLI        | `fscss in.fscss out.css` (local + network)|
| Production | Compile with CLI → ship plain CSS only    |

Official docs:  
- https://fscss.devtem.org/import  
- https://fscss.devtem.org/modular-system  
- Registry: https://fscss.devtem.org/libraries  

**Next: [Numbers and Math](./num-and-math.md)**
