# When to Use FSCSS

FSCSS isn't the right tool for every project. Here's when it shines — and when you might want something else.

## FSCSS is Great For

### 1. Custom Website Development

Building a unique website with custom designs? FSCSS gives you the flexibility to create exactly what you want:

```fscss
$brand: #2563eb;
$dark: #0f172a;

.hero {
  background: linear-gradient(135deg, $dark, $brand);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

### 2. Design Systems

Creating a reusable component library? Style stores and @define blocks make it easy:

```fscss
@define button(variant, bg, text) {
  background: @use(bg);
  color: @use(text);
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
}

.btn-primary { @button(primary, #2563eb, white); }
.btn-secondary { @button(secondary, #64748b, white); }
.btn-outline { @button(outline, transparent, #2563eb); }
```

### 3. Blog and Portfolio Sites

Simple, clean projects benefit from FSCSS's straightforward syntax:

```fscss
$font-serif: 'Georgia', serif;
$font-sans: 'Inter', sans-serif;

body {
  font-family: $font-sans;
  line-height: 1.6;
  color: #334155;
}

article {
  font-family: $font-serif;
  max-width: 720px;
  margin: 0 auto;
}
```

### 4. Learning CSS Preprocessors

FSCSS is simpler than other preprocessors. It's a great stepping stone:

- Variables work like in Sass
- Style stores are simpler than mixins
- @define is easier than @extend
- The syntax is closer to vanilla CSS

### 5. Small to Medium Projects

FSCSS's lightweight nature makes it perfect for projects that don't need a full framework:

```fscss
/* Entire stylesheet in 20 lines */
$primary: #2563eb;
$gray: #64748b;

.container { max-width: 1200px; margin: 0 auto; padding: 0 1rem; }
.btn { background: $primary; color: white; padding: 8px 16px; border-radius: 4px; }
.text-gray { color: $gray; }
```

### 6. Rapid Prototyping

Need to build a quick mockup? FSCSS's shorthand speeds up the process:

```fscss
@fun(flex-center) {
  display: flex;
  align-items: center;
  justify-content: center;
}

@fun(card) {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  padding: 1.5rem;
}

.page { @fun(flex-center); min-height: 100vh; }
.modal { @fun(card); max-width: 400px; }
```

## FSCSS Might Not Be For You If...

### 1. You Need a Full Framework

If you want pre-built components, grids, and utilities out of the box, look at established frameworks like Bootstrap or Foundation.

FSCSS is a **preprocessor**, not a framework. You build your own components.

### 2. You're Already Deep in Another Preprocessor

If your team already uses Sass/SCSS and has established patterns, switching to FSCSS might not be worth the migration cost.

### 3. You Need Advanced Features

FSCSS is lightweight by design. If you need:

- Advanced conditionals with complex logic
- Function libraries
- Extensive plugin ecosystems

Other preprocessors might be better choices.

### 4. You're Building a Very Large Enterprise App

Large teams with complex build pipelines might prefer more established tools with enterprise support.

## Quick Decision Guide

| Scenario | Recommendation |
|----------|----------------|
| Building a custom website | Use FSCSS |
| Creating a design system | Use FSCSS |
| Learning preprocessors | Use FSCSS |
| Rapid prototyping | Use FSCSS |
| Need pre-built components | Consider Bootstrap or Foundation |
| Already using another preprocessor | Stay with what works |
| Enterprise app with big team | Consider established tools |

## The Sweet Spot

FSCSS hits the sweet spot for developers who want:

- **More power than plain CSS** — Variables, stores, loops
- **Less complexity than other preprocessors** — Simpler syntax, fewer concepts
- **More control than frameworks** — Your own class names, your own patterns
- **Faster development** — Shorthand that speeds up typing

## Summary

Use FSCSS when you want to write CSS faster without the overhead of a full framework. It's perfect for custom designs, design systems, learning preprocessors, and small-to-medium projects. For large enterprise apps or when you need pre-built components, consider other tools.

The best way to know if FSCSS is for you? Try it. Build something small and see how it feels.

---

**Next: [Back to Tutorial Home](../README.md) | Start [Installation](../02-installation/README.md)**
