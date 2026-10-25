# When to Use FSCSS

FSCSS isn't the right tool for every project. Here's when it shines — and when you might want something else.

## FSCSS is Great For

### 1. Custom Website Development

Building a unique website with custom designs? FSCSS gives you the flexibility to create exactly what you want:

```css
$brand: #2563eb;
$dark: #0f172a;

.hero {
  bg: linear-gradient(135deg, $dark, $brand);
  min-h: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

### 2. Design Systems

Creating a reusable component library? Style stores and @define blocks make it easy:

```css
@define button(variant, bg, text) {
  bg: @use(bg);
  color: @use(text);
  padding: 12px 24px;
  rounded: 8px;
  font-weight: 600;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
}

.btn-primary { @button(primary, #2563eb, white) }
.btn-secondary { @button(secondary, #64748b, white) }
.btn-outline { @button(outline, transparent, #2563eb) }
```

### 3. Blog and Portfolio Sites

Simple, clean projects benefit from FSCSS's straightforward syntax:

```css
$font-serif: 'Georgia', serif;
$font-sans: 'Inter', sans-serif;

body {
  ff: $font-sans;
  line-height: 1.6;
  color: #334155;
}

article {
  ff: $font-serif;
  max-w: 720px;
  m: 0 auto;
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

```css
$primary: #2563eb;
$gray: #64748b;

.container { max-w: 1200px; margin: 0 auto; padding: 0 1rem; }
.btn { bg: $primary; color: white; padding: 8px 16px; rounded: 4px; }
.text-gray { color: $gray; }
```

### 6. Rapid Prototyping

Need to build a quick mockup? FSCSS's shorthand speeds up the process:

```css
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

.page { @fun.flex-center min-height: 100vh; }
.modal { @fun.card max-width: 400px; }
```


## Summary

Use FSCSS when you want to write CSS faster without the overhead of a full framework. It's perfect for custom designs, design systems, learning preprocessors.

The best way to know if FSCSS is for you? Try it. Build something small and see how it feels.

---

**Next: [Back to Tutorial Home](../README.md) | Start [Installation](../02-installation/README.md)**
