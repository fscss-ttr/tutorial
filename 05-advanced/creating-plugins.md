# Creating Plugins

Build reusable FSCSS extensions that others can use. Share your patterns with the community.

## What is a Plugin?

A plugin is a collection of FSCSS code that provides reusable styles, components, or utilities. It can be shared via npm or GitHub.

## Plugin Structure

```
my-fscss-plugin/
├── package.json
├── README.md
├── index.fscss
├── variables/
│   ├── colors.fscss
│   └── spacing.fscss
├── mixins/
│   ├── flexbox.fscss
│   └── typography.fscss
└── components/
    ├── button.fscss
    ├── card.fscss
    └── input.fscss
```

## package.json

```json
{
  "name": "my-fscss-plugin",
  "version": "1.0.0",
  "description": "A collection of FSCSS components and utilities",
  "main": "index.fscss",
  "keywords": ["fscss", "css", "components"],
  "author": "Your Name",
  "license": "MIT"
}
```

## Main Entry Point

```fscss
/* index.fscss */

/* Variables */
@import 'variables/colors';
@import 'variables/spacing';

/* Mixins */
@import 'mixins/flexbox';
@import 'mixins/typography';

/* Components */
@import 'components/button';
@import 'components/card';
@import 'components/input';
```

## Creating Components

### Button Component

```fscss
/* components/button.fscss */

@define button(variant, bg, text) {
  background: @use(bg);
  color: @use(text);
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  border: none;
  -*transition: all 0.2s ease;

  &:hover {
    filter: brightness(0.9);
  }

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
}

@define button-size(size, padding, font-size) {
  padding: @use(padding);
  font-size: @use(font-size);
}

/* Variants */
.btn-primary { @button(primary, #2563eb, white); }
.btn-secondary { @button(secondary, #64748b, white); }
.btn-danger { @button(danger, #dc2626, white); }
.btn-outline { @button(outline, transparent, #2563eb); border: 2px solid #2563eb; }

/* Sizes */
.btn-sm { @button-size(sm, 8px 16px, 14px); }
.btn-md { @button-size(md, 12px 24px, 16px); }
.btn-lg { @button-size(lg, 16px 32px, 18px); }
```

### Card Component

```fscss
/* components/card.fscss */

@fun(card) {
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  -*transition: transform 0.3s ease, box-shadow 0.3s ease;
}

@fun(card-hover) {
  -*transform: translateY(-4px);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);
}

@fun(card-body) {
  padding: 1.5rem;
}

@fun(card-title) {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

@fun(card-text) {
  color: #64748b;
  line-height: 1.6;
}

.card { @fun(card); }
.card:hover { @fun(card-hover); }
.card-body { @fun(card-body); }
.card-title { @fun(card-title); }
.card-text { @fun(card-text); }
```

## Creating Utilities

### Flexbox Utilities

```fscss
/* mixins/flexbox.fscss */

@fun(flex-center) {
  display: flex;
  align-items: center;
  justify-content: center;
}

@fun(flex-between) {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

@fun(flex-start) {
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

@fun(flex-end) {
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

@fun(flex-column) {
  display: flex;
  flex-direction: column;
}

@fun(flex-wrap) {
  display: flex;
  flex-wrap: wrap;
}
```

### Typography Utilities

```fscss
/* mixins/typography.fscss */

@define text-size(size) {
  font-size: @use(size);
}

@define text-weight(weight) {
  font-weight: @use(weight);
}

@define text-color(color) {
  color: @use(color);
}

.text-sm { @text-size(0.875rem); }
.text-base { @text-size(1rem); }
.text-lg { @text-size(1.125rem); }
.text-xl { @text-size(1.25rem); }
.text-2xl { @text-size(1.5rem); }
.text-3xl { @text-size(1.875rem); }

.text-light { @text-weight(300); }
.text-normal { @text-weight(400); }
.text-medium { @text-weight(500); }
.text-semibold { @text-weight(600); }
.text-bold { @text-weight(700); }
```

## Publishing Your Plugin

### 1. Test Locally

```bash
# In your project
npm install /path/to/my-fscss-plugin
```

### 2. Publish to npm

```bash
npm login
npm publish
```

### 3. Share on GitHub

Create a repository and push your code:

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/yourusername/my-fscss-plugin.git
git push -u origin main
```

## Best Practices

### 1. Document Everything

```markdown
# My FSCSS Plugin

## Installation

npm install my-fscss-plugin

## Usage

@import 'my-fscss-plugin';
```

### 2. Use Semantic Versioning

```json
{
  "version": "1.2.3"
}
```

### 3. Keep It Modular

Let users import only what they need:

```fscss
@import 'my-fscss-plugin/button';
@import 'my-fscss-plugin/card';
```

## Exercises

1. Create a button component plugin
2. Build a utility library for common patterns
3. Publish your plugin to npm

---

**Next: [FSCSS Architecture](./fscss-architecture.md)**
