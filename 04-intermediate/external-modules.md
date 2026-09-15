# External Modules

Import external FSCSS files from URLs, CDNs, or other projects. Reuse code across projects.

## Basic External Import

Import from a URL:

```fscss
@import 'https://example.com/styles/variables.fscss';
```

## CDN Imports

Import from FSCSS CDN:

```fscss
@import 'https://cdn.jsdelivr.net/gh/fscss-ttr/micros.fscss/micros.fscss';
```

## Local External Files

Import from a local path outside your project:

```fscss
@import '/path/to/shared/variables.fscss';
@import '../shared-project/mixins.fscss';
```

## NPM Packages

Import from installed npm packages:

```fscss
@import 'fscss-mixins';
@import 'fscss-components';
```

After installing:

```bash
npm install fscss-mixins
```

## Importing Specific Parts

Import only what you need:

```fscss
/* Import everything */
@import 'external-file';

/* Or use specific parts */
@import 'external-file' (variables);
@import 'external-file' (mixins);
```

## Combining Local and External

```fscss
/* External variables */
@import 'https://cdn.example.com/design-tokens.fscss';

/* Local overrides */
$primary: #2563eb;  /* Override external value */

/* Local components using both */
@import 'components/card';
@import 'components/button';
```

## Creating Shareable Modules

### Package Structure

```
my-fscss-package/
├── package.json
├── index.fscss
├── variables.fscss
├── mixins.fscss
└── components/
    ├── card.fscss
    └── button.fscss
```

### package.json

```json
{
  "name": "my-fscss-package",
  "version": "1.0.0",
  "main": "index.fscss",
  "keywords": ["fscss", "css"]
}
```

### index.fscss

```fscss
@import 'variables';
@import 'mixins';
@import 'components/card';
@import 'components/button';
```

### Publishing

```bash
npm publish
```

Now others can use:

```bash
npm install my-fscss-package
```

```fscss
@import 'my-fscss-package';
```

## Version Pinning

Pin to a specific version:

```fscss
@import 'https://cdn.jsdelivr.net/npm/my-package@1.2.3/index.fscss';
```

## Fallbacks

Provide fallback if external fails:

```fscss
/* Try external first */
@import 'https://cdn.example.com/styles.fscss';

/* Fallback to local */
@import 'local-styles';
```

## Best Practices

### 1. Version Your Packages

```json
{
  "version": "1.0.0"
}
```

### 2. Document Dependencies

```markdown
## Dependencies

- my-fscss-package@^1.0.0
```

### 3. Keep External Imports Minimal

```fscss
/* Good: Import only what you need */
@import 'external/variables';

/* Avoid: Import everything */
@import 'external/everything';
```

### 4. Use Semantic Versioning

```json
{
  "dependencies": {
    "my-fscss-package": "^1.2.3"
  }
}
```

## Practical Example

### Design System Package

```fscss
/* design-system/tokens.fscss */
$color-primary: #2563eb;
$spacing-unit: 0.25rem;
$radius-md: 8px;

/* design-system/mixins.fscss */
@fun(flex-center) {
  display: flex;
  align-items: center;
  justify-content: center;
}

/* design-system/index.fscss */
@import 'tokens';
@import 'mixins';
```

### Usage in Project

```fscss
/* Install: npm install @company/design-system */

/* In your project */
@import '@company/design-system';

.btn {
  @fun(flex-center);
  background: $color-primary;
}
```

## Exercises

1. Create a shareable FSCSS module with variables and mixins
2. Import an external FSCSS file from a CDN
3. Set up a local package for team reuse

---

**Next: [Random Values](./random-values.md)**
