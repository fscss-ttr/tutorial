# Basic Selectors

Selectors tell FSCSS which elements to style. If you know CSS selectors, you already know FSCSS selectors.

## Element Selectors

Target elements by tag name:

```fscss
h1 {
  font-size: 2rem;
  font-weight: 700;
}

p {
  line-height: 1.6;
  color: #334155;
}

a {
  color: #2563eb;
  text-decoration: none;
}
```

## Class Selectors

Target elements by class name (most common):

```fscss
.button {
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem;
}

.text-center {
  text-align: center;
}
```

## ID Selectors

Target a single element by ID:

```fscss
#header {
  background: white;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

#main-content {
  padding: 2rem 0;
}
```

## Combinators

### Descendant Selector (space)

Target elements inside other elements:

```fscss
.nav a {
  color: #334155;
  text-decoration: none;
}

.card .title {
  font-size: 1.25rem;
  font-weight: 600;
}
```

### Child Selector (`>`)

Target direct children only:

```fscss
.nav > li {
  display: inline-block;
}

.form > .input {
  margin-bottom: 1rem;
}
```

### Adjacent Sibling (`+`)

Target elements immediately after another:

```fscss
h1 + p {
  font-size: 1.125rem;
  color: #64748b;
  margin-top: 0.5rem;
}
```

### General Sibling (`~`)

Target all siblings after another:

```fscss
h1 ~ p {
  margin-top: 1rem;
}
```

## Pseudo-Classes

Target elements in specific states:

```fscss
/* Hover state */
.button:hover {
  background: #1d4ed8;
}

/* Focus state */
.input:focus {
  outline: none;
  border-color: #2563eb;
}

/* Active state */
.button:active {
  transform: scale(0.98);
}

/* First child */
.list-item:first-child {
  border-top: none;
}

/* Last child */
.list-item:last-child {
  border-bottom: none;
}

/* nth-child */
.list-item:nth-child(even) {
  background: #f8fafc;
}
```

## Pseudo-Elements

Style specific parts of elements:

```fscss
/* Before pseudo-element */
.quote::before {
  content: '"';
  font-size: 2rem;
  color: #2563eb;
}

/* After pseudo-element */
.link::after {
  content: ' →';
  -*transition: transform 0.2s ease;
}

.link:hover::after {
  transform: translateX(4px);
}

/* First letter */
.article p:first-letter {
  font-size: 2rem;
  font-weight: 700;
  float: left;
  margin-right: 0.5rem;
}
```

## Attribute Selectors

Target elements with specific attributes:

```fscss
/* Elements with href */
a[href] {
  color: #2563eb;
}

/* Links to external sites */
a[href^="https"] {
  /* external link styles */
}

/* Images */
img[alt] {
  border-radius: 8px;
}

/* Inputs with type */
input[type="text"] {
  border: 1px solid #e2e8f0;
}

input[type="email"] {
  border: 1px solid #e2e8f0;
}

input[type="submit"] {
  background: #2563eb;
  color: white;
}
```

## Specificity

CSS specificity determines which styles apply. Higher specificity wins:

```fscss
/* Low specificity */
p { color: #334155; }

/* Medium specificity */
.text { color: #64748b; }

/* High specificity */
.intro .text { color: #94a3b8; }

/* Highest specificity */
#main .intro .text { color: #1e293b; }
```

## Best Practices

### 1. Use Classes Over IDs

```fscss
/* Good */
.card { ... }
.button { ... }

/* Avoid for styling (IDs are fine for JS/anchors) */
#card { ... }
#button { ... }
```

### 2. Keep Selectors Simple

```fscss
/* Good */
.card-title { ... }

/* Avoid */
.sidebar .card .header .title { ... }
```

### 3. Use Meaningful Names

```fscss
/* Good */
.user-profile { ... }
.navigation-link { ... }

/* Avoid */
.box { ... }
.style1 { ... }
```

### 4. Avoid !important

```fscss
/* Bad */
.button {
  color: white !important;
}

/* Better - increase specificity */
.sidebar .button {
  color: white;
}
```

## Combining with FSCSS Features

### Selectors with Variables

```fscss
$primary: #2563eb;

.button {
  background: $primary;
}

.button:hover {
  background: darken($primary, 10%);
}
```

### Selectors with Style Stores

```fscss
@fun(card) {
  background: white;
  border-radius: 8px;
  padding: 1.5rem;
}

.card { @fun.card; }
.card:hover { box-shadow: 0 8px 12px rgba(0, 0, 0, 0.15); }
```

## Exercises

1. Style a navigation menu using descendant selectors
2. Create hover effects with pseudo-classes
3. Add decorative content with pseudo-elements

## Summary

FSCSS uses standard CSS selectors. Master element, class, and ID selectors, then move to combinators and pseudo-classes. Keep your selectors simple and meaningful for maintainable code.

---

**Next: [First Project](./first-project.md)**
