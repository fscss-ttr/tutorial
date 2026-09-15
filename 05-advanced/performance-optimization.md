# Performance Optimization

Write efficient FSCSS that loads fast and renders quickly.

## File Size Optimization

### 1. Remove Unused Styles

```fscss
/* Bad: Unused styles */
.unused-class { color: red; }
.another-unused { background: blue; }

/* Good: Only what you use */
.used-class { color: red; }
```

### 2. Use Shorthand

```fscss
/* Bad: Verbose */
.button {
  padding-top: 12px;
  padding-right: 24px;
  padding-bottom: 12px;
  padding-left: 24px;
}

/* Good: Shorthand */
.button {
  padding: 12px 24px;
}
```

### 3. Minimize Vendor Prefixes

```fscss
/* Use -* only when needed */
.button {
  -*transition: all 0.3s ease;  /* Modern property, needs prefix */
}

.text {
  color: red;  /* No prefix needed */
}
```

## Selector Performance

### 1. Avoid Deep Nesting

```fscss
/* Bad: Deep nesting */
.sidebar .nav .menu .item .link {
  color: blue;
}

/* Good: Flat selectors */
.nav-link {
  color: blue;
}
```

### 2. Use Classes Over Elements

```fscss
/* Bad: Element selectors */
div > p > a {
  color: blue;
}

/* Good: Class selectors */
.link {
  color: blue;
}
```

### 3. Avoid Universal Selectors

```fscss
/* Bad: Universal selector */
* {
  box-sizing: border-box;
}

/* Good: Specific selectors */
.card, .button, .input {
  box-sizing: border-box;
}
```

## Rendering Performance

### 1. Avoid Expensive Properties

```fscss
/* Bad: Expensive */
.box {
  filter: blur(10px);
  backdrop-filter: blur(10px);
}

/* Good: Use sparingly */
.box {
  filter: blur(5px);  /* Less blur */
}
```

### 2. Use Transform Over Layout Properties

```fscss
/* Bad: Triggers layout */
.box {
  left: 100px;
  top: 50px;
}

/* Good: GPU accelerated */
.box {
  -*transform: translate(100px, 50px);
}
```

### 3. Optimize Animations

```fscss
/* Bad: Animating expensive properties */
.box {
  -*transition: width 0.3s ease, height 0.3s ease;
}

/* Good: Animate transform and opacity */
.box {
  -*transition: transform 0.3s ease, opacity 0.3s ease;
}
```

## Loading Performance

### 1. Critical CSS

Inline critical styles:

```html
<style>
  /* Critical CSS inlined */
  .header { background: white; padding: 1rem; }
  .hero { min-height: 100vh; }
</style>

<!-- Non-critical loaded async -->
<link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
```

### 2. Preload Important Files

```html
<link rel="preload" href="fonts.woff2" as="font" type="font/woff2" crossorigin>
<link rel="preload" href="critical.css" as="style">
```

### 3. Minify CSS

```bash
# Build minified CSS
fscss src/styles.fscss dist/styles.min.css --minify
```

## FSCSS-Specific Optimizations

### 1. Efficient Variables

```fscss
/* Good: Reuse variables */
$primary: #2563eb;
$spacing: 1rem;

.button {
  background: $primary;
  padding: $spacing;
}

.card {
  border-color: $primary;
  padding: $spacing;
}

/* Bad: Inline values */
.button {
  background: #2563eb;
  padding: 1rem;
}

.card {
  border-color: #2563eb;
  padding: 1rem;
}
```

### 2. Efficient @fun

```fscss
/* Good: Single @fun for repeated patterns */
@fun(card) {
  background: white;
  border-radius: 8px;
  padding: 1.5rem;
}

.card-1 { @fun(card); }
.card-2 { @fun(card); }
.card-3 { @fun(card); }

/* Bad: Repeated properties */
.card-1 { background: white; border-radius: 8px; padding: 1.5rem; }
.card-2 { background: white; border-radius: 8px; padding: 1.5rem; }
.card-3 { background: white; border-radius: 8px; padding: 1.5rem; }
```

### 3. Efficient @define

```fscss
/* Good: Parameterized components */
@define button(bg, text) {
  background: @use(bg);
  color: @use(text);
  padding: 12px 24px;
  border-radius: 8px;
}

.btn-primary { @button(#2563eb, white); }
.btn-danger { @button(#dc2626, white); }
```

## Measuring Performance

### 1. CSS File Size

```bash
# Check file size
ls -lh dist/styles.css

# Minify and check
fscss src/styles.fscss dist/styles.min.css --minify
ls -lh dist/styles.min.css
```

### 2. Browser DevTools

1. Open DevTools (F12)
2. Go to Network tab
3. Reload page
4. Check CSS file size and load time

### 3. Lighthouse

Run Lighthouse audit for performance recommendations.

## Best Practices

### 1. Write Only What You Need

```fscss
/* Good: Minimal, focused styles */
.btn {
  background: $primary;
  color: white;
}

/* Avoid: Over-engineered styles */
.btn-primary-large-outlined-with-shadow {
  /* ... */
}
```

### 2. Use Efficient Selectors

```fscss
/* Good */
.card { ... }
.card-title { ... }

/* Bad */
div.card > h3.title { ... }
```

### 3. Optimize for Mobile

```fscss
/* Mobile-first */
.container {
  padding: 1rem;
}

/* Desktop enhancements */
@media (min-width: 768px) {
  .container {
    padding: 2rem;
    max-width: 1200px;
  }
}
```

## Exercises

1. Optimize a large stylesheet for file size
2. Improve selector performance
3. Set up critical CSS inlining

---

**Next: [Contributing to FSCSS](./contributing-to-fscss.md)**
