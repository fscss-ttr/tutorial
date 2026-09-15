# Enterprise Applications

Scale FSCSS for large teams and enterprise applications.

## Enterprise Challenges

### 1. Large Codebases

- Thousands of FSCSS files
- Multiple teams working simultaneously
- Complex dependency graphs

### 2. Consistency

- Design system enforcement
- Code standards
- Review processes

### 3. Performance

- Fast compilation
- Optimized output
- Caching strategies

## Enterprise Architecture

### Monorepo Structure

```
enterprise/
├── packages/
│   ├── design-system/
│   │   ├── src/
│   │   │   ├── tokens/
│   │   │   ├── mixins/
│   │   │   └── components/
│   │   └── package.json
│   ├── web-app/
│   │   ├── src/
│   │   └── package.json
│   └── admin-app/
│       ├── src/
│       └── package.json
├── tools/
│   ├── fscss-lint/
│   └── fscss-build/
├── package.json
└── lerna.json
```

### Design System Package

```fscss
/* packages/design-system/src/tokens.fscss */
$primary: #2563eb;
$spacing-unit: 0.25rem;
$radius-md: 8px;

/* packages/design-system/src/mixins.fscss */
@fun(flex-center) {
  display: flex;
  align-items: center;
  justify-content: center;
}

/* packages/design-system/src/index.fscss */
@import 'tokens';
@import 'mixins';
```

### Application Usage

```fscss
/* packages/web-app/src/styles.fscss */
@import '@enterprise/design-system';

.page {
  @fun(flex-center);
  min-height: 100vh;
  padding: $spacing-unit * 8;
}
```

## Build Pipeline

### Turborepo Configuration

```json
// turbo.json
{
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**"]
    },
    "fscss": {
      "outputs": ["dist/css/**"]
    }
  }
}
```

### Build Script

```json
// packages/web-app/package.json
{
  "scripts": {
    "fscss": "fscss src/styles.fscss dist/css/styles.css",
    "build": "npm run fscss && next build"
  }
}
```

## Code Standards

### ESLint for FSCSS

```json
// .eslintrc.json
{
  "plugins": ["fscss"],
  "rules": {
    "fscss/no-unused-variables": "error",
    "fscss/consistent-naming": "error",
    "fscss/no-hardcoded-values": "warn"
  }
}
```

### Stylelint Configuration

```json
// .stylelintrc.json
{
  "extends": "stylelint-config-standard",
  "rules": {
    "color-hex-length": "short",
    "declaration-block-no-redundant-longhand-properties": true,
    "no-descending-specificity": true
  }
}
```

## Performance Optimization

### Parallel Compilation

```javascript
// build.js
const { execSync } = require('child_process');
const glob = require('glob');

const files = glob.sync('src/**/*.fscss');

files.forEach(file => {
  const output = file.replace('.fscss', '.css');
  execSync(`fscss ${file} ${output}`);
});
```

### Caching

```javascript
// cache.js
const fs = require('fs');
const crypto = require('crypto');

function getHash(file) {
  const content = fs.readFileSync(file);
  return crypto.createHash('md5').update(content).digest('hex');
}

function shouldRecompile(file, cacheFile) {
  if (!fs.existsSync(cacheFile)) return true;
  
  const cached = JSON.parse(fs.readFileSync(cacheFile));
  return cached.hash !== getHash(file);
}
```

## CI/CD for Enterprise

### GitHub Actions

```yaml
# .github/workflows/enterprise.yml
name: Enterprise Build

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Lint FSCSS
        run: npm run lint:fscss
      
      - name: Build packages
        run: npm run build
      
      - name: Run tests
        run: npm test
```

## Best Practices

### 1. Design Tokens First

Define all tokens in one place, reference everywhere.

### 2. Component Library

Build reusable components, not just styles.

### 3. Documentation

Document everything for team onboarding.

### 4. Automated Testing

Test styles as rigorously as JavaScript.

## Exercises

1. Set up a monorepo with FSCSS
2. Create a design system package
3. Implement automated linting

---

**Next: [Future of FSCSS](./future-of-fscss.md)**
