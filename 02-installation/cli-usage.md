# CLI Usage

The FSCSS command-line tool lets you compile `.fscss` files to `.css` from your terminal.

## Basic Usage

Compile a single file:

```bash
fscss input.fscss output.css
```

Example:

```bash
fscss src/styles.fscss dist/styles.css
```

## Command Options

| Option | Description | Example |
|--------|-------------|---------|
| `input` | Source `.fscss` file | `src/styles.fscss` |
| `output` | Destination `.css` file | `dist/styles.css` |
| `--watch` | Watch for changes | `fscss --watch src.fscss dist.css` |
| `--version` | Show version | `fscss --version` |
| `--help` | Show help | `fscss --help` |

## Watch Mode

Watch a file for changes and recompile automatically:

```bash
fscss --watch src/styles.fscss dist/styles.css
```

Press `Ctrl+C` to stop watching.

## Compiling Multiple Files

Compile several files at once:

```bash
fscss src/variables.fscss dist/variables.css
fscss src/components.fscss dist/components.css
fscss src/layout.fscss dist/layout.css
```

Or use a shell script:

```bash
#!/bin/bash
fscss src/variables.fscss dist/variables.css
fscss src/components.fscss dist/components.css
fscss src/layout.fscss dist/layout.css
```

Save as `build.sh` and run:

```bash
chmod +x build.sh
./build.sh
```

## Project Setup

### Recommended Project Structure

```
my-project/
├── src/
│   └── styles/
│       ├── variables.fscss
│       ├── components.fscss
│       └── main.fscss
├── dist/
│   └── styles/
│       ├── variables.css
│       ├── components.css
│       └── main.css
├── package.json
└── index.html
```

### Adding npm Scripts

Add these scripts to your `package.json`:

```json
{
  "scripts": {
    "build:css": "fscss src/styles/main.fscss dist/styles/main.css",
    "watch:css": "fscss --watch src/styles/main.fscss dist/styles/main.css",
    "build:all": "npm run build:css"
  }
}
```

### Running Scripts

```bash
# Build CSS once
npm run build:css

# Watch for changes
npm run watch:css

# Build all files
npm run build:all
```

## Working with Imports

FSCSS supports imports. If your main file imports others:

```fscss
/* main.fscss */
@import 'variables';
@import 'components';
@import 'layout';
```

Compile the main file:

```bash
fscss src/styles/main.fscss dist/styles/main.css
```

FSCSS automatically handles the imports.

## Error Handling

If there's an error in your FSCSS file, the CLI will show:

```
Error: Unexpected token at line 15
  |
15 | .button { background: }
  |                     ^ Unexpected token '}'
```

Fix the error and run the command again.

## Useful Commands

### Check Version

```bash
fscss --version
```

### Show Help

```bash
fscss --help
```

### Get File Info

```bash
fscss --info
```

## Integration with Build Tools

### Webpack

Create a simple webpack plugin or use a loader.

### Gulp

```javascript
const { exec } = require('child_process');

function compileCSS() {
  return exec('fscss src/styles.fscss dist/styles.css');
}

exports.default = compileCSS;
```

### Package.json Scripts

For most projects, npm scripts are sufficient:

```json
{
  "scripts": {
    "dev": "fscss --watch src/styles.fscss dist/styles.css",
    "build": "fscss src/styles.fscss dist/styles.css"
  }
}
```

## Summary

The FSCSS CLI is simple and effective. Use it for single file compilation, watch mode, and integration with build scripts. For more complex workflows, combine it with npm scripts or task runners.

---

**Next: [Multi-Stack Setup](./using-in-python-rust-php.md)**
