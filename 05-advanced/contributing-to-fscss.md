# Contributing to FSCSS

Help improve FSCSS by contributing code, documentation, or bug reports.

## Ways to Contribute

### 1. Report Bugs

Found a bug? Create an issue:

1. Go to [GitHub Issues](https://github.com/fscss-ttr/FSCSS/issues)
2. Click "New Issue"
3. Describe the bug clearly:
   - What you expected
   - What actually happened
   - Steps to reproduce
   - FSCSS version

### 2. Suggest Features

Have an idea? Share it:

1. Go to [GitHub Discussions](https://github.com/fscss-ttr/FSCSS/discussions)
2. Start a new discussion
3. Describe your feature idea
4. Explain why it would be useful

### 3. Improve Documentation

Help others learn FSCSS:

- Fix typos
- Add examples
- Clarify explanations
- Translate to other languages

### 4. Submit Code

Fix bugs or add features:

1. Fork the repository
2. Create a branch
3. Make your changes
4. Submit a pull request

## Setting Up for Development

### 1. Fork the Repository

Go to [FSCSS GitHub](https://github.com/fscss-ttr/FSCSS) and click "Fork".

### 2. Clone Your Fork

```bash
git clone https://github.com/yourusername/FSCSS.git
cd FSCSS
```

### 3. Install Dependencies

```bash
npm install
```

### 4. Create a Branch

```bash
git checkout -b feature/your-feature-name
```

## Making Changes

### 1. Understand the Codebase

```
FSCSS/
├── src/           # Source code
├── tests/         # Tests
├── docs/          # Documentation
├── package.json
└── README.md
```

### 2. Follow Code Style

- Use consistent indentation
- Follow naming conventions
- Add comments for complex logic

### 3. Write Tests

```javascript
// tests/example.test.js
const { test, expect } = require('@jest/globals');

test('variable compilation', () => {
  const input = '$primary: #2563eb; .btn { background: $primary; }';
  const output = fscss(input);
  expect(output).toContain('.btn { background: #2563eb; }');
});
```

### 4. Update Documentation

If your change affects usage, update the docs:

- Add examples
- Explain new features
- Update existing documentation

## Submitting a Pull Request

### 1. Commit Your Changes

```bash
git add .
git commit -m "Add: new feature description"
```

Use clear commit messages:
- `Add:` for new features
- `Fix:` for bug fixes
- `Update:` for improvements
- `Docs:` for documentation

### 2. Push to GitHub

```bash
git push origin feature/your-feature-name
```

### 3. Create Pull Request

1. Go to your fork on GitHub
2. Click "New Pull Request"
3. Select your branch
4. Describe your changes
5. Submit

## Code of Conduct

- Be respectful
- Welcome newcomers
- Focus on the code
- Accept feedback gracefully

## Getting Help

- [GitHub Discussions](https://github.com/fscss-ttr/FSCSS/discussions)
- [Discord Community](https://discord.gg/fscss)
- [Twitter @fscss-ttr](https://twitter.com/fscss-ttr)

## Exercises

1. Set up FSCSS for development
2. Find and fix a bug
3. Improve documentation for a feature

---

**Next: [Migration from Sass](./migration-from-sass.md)**
