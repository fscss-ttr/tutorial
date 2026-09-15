# npm Setup

The most common way to install FSCSS is via npm. This works for any Node.js project.

## Local Installation (Recommended)

Install FSCSS as a project dependency:

```bash
npm install fscss@latest
```

This adds FSCSS to your `package.json` and `node_modules` folder.

### Project Structure

After installation, your project should look like this:

```
my-project/
├── node_modules/
│   └── fscss/
├── package.json
├── package-lock.json
├── style.fscss
└── index.html
```

### Using FSCSS in Your Project

Create an `.fscss` file and compile it:

```bash
npx fscss style.fscss style.css
```

Or add a script to your `package.json`:

```json
{
  "scripts": {
    "build:css": "fscss src/styles.fscss dist/styles.css",
    "watch:css": "fscss --watch src/styles.fscss dist/styles.css"
  }
}
```

Then run:

```bash
npm run build:css
```

## Global Installation

Install FSCSS globally for use across all projects:

```bash
npm install -g fscss
```

Now you can use the `fscss` command anywhere:

```bash
fscss input.fscss output.css
```

## Development Dependencies

For production projects, install as a dev dependency:

```bash
npm install --save-dev fscss@latest
```

This keeps FSCSS out of your production bundle.

## Updating FSCSS

Update to the latest version:

```bash
npm update fscss
```

Or reinstall:

```bash
npm uninstall fscss
npm install fscss@latest
```

## Uninstalling

Remove FSCSS from your project:

```bash
npm uninstall fscss
```

## Troubleshooting

### Command Not Found

If `fscss` isn't recognized after global install:

1. Check your PATH environment variable
2. Try using `npx fscss` instead
3. Reinstall globally with `npm install -g fscss`

### Permission Errors

On macOS/Linux, you might need:

```bash
sudo npm install -g fscss
```

Or fix npm permissions:

```bash
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
export PATH=~/.npm-global/bin:$PATH
```

### Version Conflicts

If you have multiple versions installed:

```bash
# Check which version is being used
which fscss

# Use a specific version
npx fscss@1.2.0 input.fscss output.css
```

## Summary

npm is the recommended way to install FSCSS for Node.js projects. Use local installation for project-specific work, or global installation for command-line access across projects.

---

**Next: [CDN Browser](./cdn-browser.md)**
