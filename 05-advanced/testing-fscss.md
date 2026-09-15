# Testing FSCSS

Test your FSCSS code to catch errors and ensure consistency.

## Why Test FSCSS?

- Catch syntax errors early
- Ensure consistent output
- Prevent regressions
- Validate design tokens

## Testing Approaches

### 1. Compile and Check Output

```bash
# Compile and verify
fscss src/styles.fscss dist/styles.css

# Check file exists
ls -la dist/styles.css

# Verify file size
wc -c dist/styles.css
```

### 2. Visual Testing

Open compiled CSS in a browser and verify visually.

### 3. Automated Testing

Use testing frameworks to validate output.

## Basic Test Script

```bash
#!/bin/bash
# test-fscss.sh

echo "Compiling FSCSS..."

# Compile
fscss src/styles.fscss dist/styles.css

# Check if compilation succeeded
if [ $? -eq 0 ]; then
  echo "✓ Compilation successful"
else
  echo "✗ Compilation failed"
  exit 1
fi

# Check file size
SIZE=$(wc -c < dist/styles.css)
echo "✓ File size: $SIZE bytes"

# Check for errors
if grep -q "error" dist/styles.css; then
  echo "✗ Errors found in output"
  exit 1
else
  echo "✓ No errors in output"
fi

echo "All tests passed!"
```

## Jest Testing

### Setup

```bash
npm install --save-dev jest
```

### Test File

```javascript
// tests/styles.test.js
const fs = require('fs');
const { execSync } = require('child_process');

describe('FSCSS Compilation', () => {
  beforeAll(() => {
    execSync('fscss src/styles.fscss dist/styles.css');
  });

  test('compiles successfully', () => {
    const css = fs.readFileSync('dist/styles.css', 'utf8');
    expect(css).toBeDefined();
  });

  test('contains expected classes', () => {
    const css = fs.readFileSync('dist/styles.css', 'utf8');
    expect(css).toContain('.button');
    expect(css).toContain('.card');
  });

  test('does not contain FSCSS syntax', () => {
    const css = fs.readFileSync('dist/styles.css', 'utf8');
    expect(css).not.toContain('$primary');
    expect(css).not.toContain('@fun');
  });
});
```

### Run Tests

```bash
npm test
```

## Stylelint

### Install

```bash
npm install --save-dev stylelint stylelint-config-standard
```

### Configuration

```json
// .stylelintrc.json
{
  "extends": "stylelint-config-standard",
  "rules": {
    "color-hex-length": "short",
    "declaration-block-no-redundant-longhand-properties": true,
    "no-duplicate-selectors": true
  }
}
```

### Run

```bash
npx stylelint dist/styles.css
```

## Visual Regression Testing

### Percy

```javascript
// percy.config.js
module.exports = {
  version: 1,
  snapshot: [
    {
      name: 'Button Variants',
      url: 'http://localhost:3000/buttons.html',
    },
    {
      name: 'Card Components',
      url: 'http://localhost:3000/cards.html',
    },
  ],
};
```

### BackstopJS

```json
// backstop.json
{
  "id": "fscss-tests",
  "viewports": [
    { "label": "phone", "width": 320 },
    { "label": "tablet", "width": 768 },
    { "label": "desktop", "width": 1024 }
  ],
  "scenarios": [
    {
      "label": "Button",
      "url": "http://localhost:3000/button.html"
    }
  ]
}
```

## CI/CD Integration

### GitHub Actions

```yaml
# .github/workflows/test.yml
name: Test FSCSS

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm install -g fscss
      - run: fscss src/styles.fscss dist/styles.css
      - run: npm test
```

## Best Practices

### 1. Test Before Commit

```bash
# Add to pre-commit hook
fscss src/styles.fscss dist/styles.css && npm test
```

### 2. Test on Multiple Browsers

Use BrowserStack or Sauce Labs for cross-browser testing.

### 3. Visual Testing

Compare screenshots before and after changes.

## Exercises

1. Set up basic FSCSS testing
2. Create Jest tests for your styles
3. Add Stylelint to your project

---

**Next: [CI/CD Integration](./ci-cd-integration.md)**
