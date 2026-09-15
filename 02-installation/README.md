# Installation

Get FSCSS up and running in minutes. Choose the method that works best for your workflow.

## Installation Methods

| Method | Best For | Complexity |
|--------|----------|------------|
| [npm Setup](./npm-setup.md) | Node.js projects | Easy |
| [CDN Browser](./cdn-browser.md) | Quick testing, HTML pages | Very Easy |
| [CLI Usage](./cli-usage.md) | Build scripts, command line | Easy |
| [Multi-Stack Setup](./using-in-python-rust-php.md) | Python, Rust, PHP projects | Medium |

## Prerequisites

Before installing FSCSS, make sure you have:

- **Node.js** (v14 or higher) — Required for npm installation
- **A code editor** — VS Code recommended
- **Terminal/Command line** — For running FSCSS commands

## Quick Install

The fastest way to get started:

```bash
npm install fscss@latest
```

Or use the CDN for instant access:

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.1/runtime.min.js" async></script>
```

## Verify Installation

After installing, verify FSCSS is working:

```bash
npm view fscss version
```

You should see the version number printed to your terminal.

## Which Method Should I Choose?

| If you are... | Use this method |
|---------------|-----------------|
| Building a Node.js project | npm Setup |
| Testing FSCSS quickly | CDN Browser |
| Working with build scripts | CLI Usage |
| Using Python, Rust, or PHP | Multi-Stack Setup |

---

**Next: [npm Setup](./npm-setup.md)**

---
