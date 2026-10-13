# Testing FSCSS

FSCSS has no test runner — testing means *compiling and checking the output*, which is exactly the right thing to test. The compiled CSS is deterministic (ignore `@random`), so you can assert against it directly.

## Test 1: It Compiles

The cheapest test in the world, and it catches 90% of damage:

```bash
fscss src/styles.fscss dist/styles.css
```

Exit code `0` means the source is valid FSCSS. Failures print token locations. Make this the first gate in CI.

## Test 2: Compile Against Known Output

Compile, then diff against a committed golden file:

```bash
fscss src/styles.fscss /tmp/current.css
diff /tmp/current.css dist/styles.golden.css
```

Golden tests are strict but brittle on purpose: anything that changes declarative output (a new token, an `@arr` order flip) surfaces instantly. Keep goldens per module, not as one giant file.

## Test 3: Assert on the Output

A tiny Node script that hammers your guarantees:

```js
const fs = require('fs');
const css = fs.readFileSync('dist/styles.css', 'utf8');

const checks = [
  ['.mt-2', 'margin-top: 8px'],
  ['@keyframes rise', '@keyframes rise'],
  [':root[data-theme="dark"]', 'data-theme="dark"'],
];

let fail = 0;
for (const [needle, label] of checks) {
  if (!css.includes(needle)) { console.error(`MISSING: ${label}`); fail++; }
}
process.exit(fail ? 1 : 0);
```

```json
{ "scripts": { "test:css": "npm run build:css && node test/css.test.js" } }
```

## Test 4: Use `exec()` as Flight Recorder

Temporary in-source tracing is fine during development:

```fscss
exec(_log, 'button module compiled')
exec(_warn, 'using legacy token $brand-primary!')
```

Keep warnings deliberate; delete noisy logs before merge.

## Test 5: The Reduced-Motion and Theme Matrix

Theming has no test framework, but it has *compositions*. Verify each theme compiles the same source cleanly:

```fscss
@theme-tokens([data-theme="dark"])
```

Compile the same entry several times with different scope arguments and diff outputs — a token that vanished in dark mode shows up as a difference.

## What To Assert

| Layer | Check |
|-------|-------|
| Compiles | CLI exit 0 |
| Output shape | Golden diff or `includes` assertions |
| Theme coverage | Compile-with-scope variants |
| Token integrity | Every `var(--x)` token is declared somewhere |
| Scale math | `.mt-6 { margin-top: 24px; }` literally present |

## A Healthy Pipeline

```yaml
# ci.yml
- run: npm install
- run: npm run build:css
- run: node test/css.test.js
```

Three steps. Fast, no framework, catches the regression signals that matter.

## Summary

- Testing FSCSS = compiling and asserting on the compiled CSS.
- Golden files per module catch accidental output changes.
- Assert on tokens, keyframes, themes, and generated math.
- `exec()` traces builds during debugging; CI keeps the compile green.

---

**Next: [Migrating from Sass](./migration-from-sass.md)**