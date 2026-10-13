# FSCSS with Frameworks

FSCSS doesn't care which framework you use — it compiles `.fscss` to `.css`, and every framework can link a stylesheet. The work is simply wiring compilation into your build. Mechanism: add an npm script, run FSCSS, import the result.

## The Universal Pattern

```json
{
  "scripts": {
    "build:css": "fscss src/styles.fscss dist/styles.css"
  }
}
```

Then import the compiled file wherever your app imports CSS.

## React (Vite)

Vite already imports CSS. Add the compile step, then import the output:

```json
{
  "scripts": {
    "build": "npm run build:css && vite build",
    "dev": "npm run build:css && vite",
    "build:css": "fscss src/styles.fscss src/styles.css"
  }
}
```

```jsx
// main.jsx
import './styles.css'
```

In dev, re-run `npm run build:css` when you touch FSCSS (or use a watch loop in your terminal — the FSCSS VS Code extension can help in Chrome DevTools workflows).

## React (Create React App / Webpack)

CRA treats any imported CSS as a module. Same deal:

```json
{
  "scripts": {
    "build": "npm run build:css && react-scripts build",
    "build:css": "fscss src/styles.fscss src/index.css"
  }
}
```

```jsx
import './index.css';
```

## Next.js

Next imports global CSS from `app/globals.css`. Compile into it:

```json
{
  "scripts": {
    "build": "npm run build:css && next build",
    "dev": "npm run build:css && next dev",
    "build:css": "fscss src/styles.fscss app/globals.css"
  }
}
```

Don't write FSCSS inside `globals.css` itself — it gets compiled over. Keep source in `src/`.

## Vue

Vue Single-File Components want a global stylesheet plus scoped styles. Global first:

```json
{
  "scripts": {
    "build:css": "fscss src/styles.fscss src/assets/styles.css"
  }
}
```

```js
// main.js
import './assets/styles.css'
```

For scoped styles, the pragmatic choice is ordinary CSS scoping with FSCSS-shorthand *values* — or keep scoped design tokens in your global FSCSS and reference them via `var()`:

```vue
<style scoped>
  .card { background: var(--kit-color-primary); border-radius: 8px; }
</style>
```

## The Dev Loop Without a Watcher

If your bundler doesn't watch `.fscss` files (most don't out of the box), a tiny helper keeps dev smooth:

```bash
# terminal 1 — watch and recompile
while true; do fscss src/styles.fscss src/styles.css; sleep 1; done
```

Robust version — use a watcher tool or the VS Code extension. The key idea is the same everywhere: compile happens before the bundler sees your CSS.

## Server-Side and CLI Runtime

Any backend that can run a command can compile. FastAPI, Flask, Laravel, Rust builds — see [Installation → Multi-Stack](../02-installation/using-in-python-rust-php.md) for subprocess and build-script recipes.

## Checklist

1. FSCSS compiles before the framework bundles.
2. Compiled CSS is what gets imported, never raw `.fscss`.
3. Add `build:css` to every build/dev script.
4. Framework-agnostic theming via `var(--token)` in CSS modules / scoped styles.

## Summary

- Wire `fscss in.fscss out.css` into `package.json` scripts.
- Import the compiled output in React, Vite, Next, or Vue entry points.
- Watch with a loop or the VS Code extension in dev.
- Cascade tokens through `var()` so components stay framework-neutral.

---

**Next: [Testing FSCSS](./testing-fscss.md)**