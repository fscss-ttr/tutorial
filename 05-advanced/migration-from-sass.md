# Migration from Sass

Switch from Sass/SCSS to FSCSS with this step-by-step guide.

## Why Migrate?

| Feature | Sass/SCSS | FSCSS |
|---------|-----------|-------|
| Learning curve | Steep | Simple |
| Syntax complexity | High | Low |
| Setup requirements | Complex | Minimal |
| Shorthand support | Limited | Extensive |

## Migration Steps

### 1. Install FSCSS

```bash
npm install fscss@latest
```

### 2. Find Equivalent Features

#### Variables

**Sass:**
```scss
$primary: #2563eb;
$spacing: 1rem;

.button {
  background: $primary;
  padding: $spacing;
}
```

**FSCSS:**
```fscss
$primary: #2563eb;
$spacing: 1rem;

.button {
  background: $primary;
  padding: $spacing;
}
```

Same syntax! Variables work identically.

#### Mixins

**Sass:**
```scss
@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

.centered {
  @include flex-center;
}
```

**FSCSS:**
```fscss
@fun(flex-center) {
  display: flex;
  align-items: center;
  justify-content: center;
}

.centered {
  @fun(flex-center);
}
```

#### Parameterized Mixins

**Sass:**
```scss
@mixin button($bg, $text) {
  background: $bg;
  color: $text;
  padding: 12px 24px;
  border-radius: 8px;
}

.btn-primary {
  @include button(#2563eb, white);
}
```

**FSCSS:**
```fscss
@define button(bg, text) {
  background: @use(bg);
  color: @use(text);
  padding: 12px 24px;
  border-radius: 8px;
}

.btn-primary {
  @button(#2563eb, white);
}
```

#### Nesting

**Sass:**
```scss
.card {
  background: white;

  &-title {
    font-size: 1.25rem;
  }

  &-body {
    padding: 1.5rem;
  }
}
```

**FSCSS:**
```fscss
.card {
  background: white;
}

.card-title {
  font-size: 1.25rem;
}

.card-body {
  padding: 1.5rem;
}
```

FSCSS doesn't support nesting. Write selectors explicitly.

#### Imports

**Sass:**
```scss
@import 'variables';
@import 'mixins';
```

**FSCSS:**
```fscss
@import 'variables';
@import 'mixins';
```

Same syntax!

### 3. Convert File by File

1. Start with variables (same syntax)
2. Convert mixins to @fun or @define
3. Flatten nested selectors
4. Update any Sass-specific features

### 4. Test Thoroughly

```bash
# Compile both versions
sass src/styles.scss output-sass.css
fscss src/styles.fscss output-fscss.css

# Compare output
diff output-sass.css output-fscss.css
```

## Common Conversions

### Extend

**Sass:**
```scss
%button-base {
  padding: 12px 24px;
  border-radius: 8px;
}

.btn {
  @extend %button-base;
  background: #2563eb;
}
```

**FSCSS:**
```fscss
@fun(button-base) {
  padding: 12px 24px;
  border-radius: 8px;
}

.btn {
  @fun(button-base);
  background: #2563eb;
}
```

### Maps

**Sass:**
```scss
$colors: (
  primary: #2563eb,
  secondary: #64748b
);

.button {
  background: map-get($colors, primary);
}
```

**FSCSS:**
```fscss
$primary: #2563eb;
$secondary: #64748b;

.button {
  background: $primary;
}
```

### Functions

**Sass:**
```scss
@function double($value) {
  @return $value * 2;
}

.box {
  width: double(50px);
}
```

**FSCSS:**
```fscss
@define double(value) {
  width: @use(value) * 2;
}

.box {
  @double(50px);
}
```

## Tips

1. **Start small** — Convert one file at a time
2. **Keep both** — Run Sass and FSCSS in parallel during migration
3. **Test often** — Verify output matches
4. **Update build scripts** — Replace Sass commands with FSCSS

## Exercises

1. Convert a Sass variable file to FSCSS
2. Transform mixins to @fun/@define
3. Migrate a complete component

---

**Next: [FSCSS in React](./fscss-in-react.md)**
