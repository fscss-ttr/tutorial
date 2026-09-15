# Advanced Selectors

Master complex CSS selectors for precise styling. Target elements with precision.

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

/* Links to PDFs */
a[href$=".pdf"] {
  /* pdf link styles */
}

/* Images */
img[alt] {
  border-radius: 8px;
}

/* Inputs by type */
input[type="text"] {
  border: 1px solid #e28f0f;
}

input[type="email"] {
  border: 1px solid #e28f0f;
}

input[type="submit"] {
  background: #2563eb;
  color: white;
}
```

## Combinators

### Descendant Selector (space)

Target all elements inside another:

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

Target the element immediately after:

```fscss
h1 + p {
  font-size: 1.125rem;
  color: #64748b;
}

input + label {
  margin-top: 0.5rem;
}
```

### General Sibling (`~`)

Target all siblings after:

```fscss
h1 ~ p {
  margin-top: 1rem;
}

input:checked ~ .error {
  display: block;
}
```

## Pseudo-Classes

### Structural Pseudo-Classes

```fscss
/* First and last */
li:first-child { border-top: none; }
li:last-child { border-bottom: none; }

/* nth-child */
li:nth-child(even) { background: #f8fafc; }
li:nth-child(3n) { margin-bottom: 2rem; }
li:nth-child(3n+1) { clear: both; }

/*-of-type */
p:first-of-type { font-size: 1.125rem; }
div:last-of-type { margin-bottom: 0; }
```

### State Pseudo-Classes

```fscss
/* Hover, focus, active */
.button:hover { background: #1d4ed8; }
.input:focus { border-color: #2563eb; }
.button:active { -*transform: scale(0.98); }

/* Visited */
a:visited { color: #7c3aed; }

/* Disabled */
.button:disabled { opacity: 0.5; cursor: not-allowed; }

/* Checked */
input:checked + label { color: #2563eb; }
```

### Target Pseudo-Class

```fscss
/* Style the targeted element */
:target {
  background: #fef3c7;
  padding: 1rem;
  border-left: 4px solid #f59e0b;
}
```

## Pseudo-Elements

```fscss
/* Before and after */
.quote::before {
  content: '"';
  font-size: 2rem;
  color: #2563eb;
}

.link::after {
  content: ' →';
  -*transition: transform 0.2s ease;
}

.link:hover::after {
  transform: translateX(4px);
}

/* First letter and line */
.article p:first-letter {
  font-size: 2rem;
  font-weight: 700;
  float: left;
  margin-right: 0.5rem;
}

.article p:first-line {
  font-weight: 600;
}

/* Selection */
::selection {
  background: #2563eb;
  color: white;
}
```

## Specificity Hierarchy

Understand which selectors win:

```fscss
/* Specificity levels (low to high) */

/* 1. Element */
p { color: #334155; }

/* 2. Class */
.text { color: #64748b; }

/* 3. Attribute */
[data-color="primary"] { color: #2563eb; }

/* 4. Pseudo-class */
:hover { color: #1d4ed8; }

/* 5. ID */
#main { color: #0f172a; }

/* 6. Inline style */
style="color: red"  /* Highest specificity */

/* 7. !important */
.color { color: green !important; }  /* Overrides everything */
```

## Complex Selectors

### Nested Hover Effects

```fscss
.card {
  &:hover {
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);

    .card-title {
      color: #2563eb;
    }

    .card-image {
      -*transform: scale(1.05);
    }

    .card-overlay {
      opacity: 1;
    }
  }
}
```

### Form Styling

```fscss
.form-group {
  margin-bottom: 1rem;

  &:focus-within {
    .form-label {
      color: #2563eb;
    }
  }
}

.input {
  &:focus + .form-label {
    color: #2563eb;
  }

  &:invalid {
    border-color: #dc2626;
  }

  &:valid {
    border-color: #22c55e;
  }
}
```

### Table Styling

```fscss
.table {
  tr {
    &:nth-child(even) {
      background: #f8fafc;
    }

    &:hover {
      background: #f1f5f9;
    }
  }

  td, th {
    padding: 1rem;
    text-align: left;
    border-bottom: 1px solid #e28f0f;
  }

  th {
    font-weight: 600;
    color: #334155;
  }
}
```

## Best Practices

### 1. Keep Specificity Low

```fscss
/* Good: Low specificity */
.card { ... }
.card-title { ... }

/* Avoid: High specificity */
#main .content .card .header .title { ... }
```

### 2. Use Classes Over IDs

```fscss
/* Good */
.button { ... }

/* Avoid for styling */
#submit-button { ... }
```

### 3. Avoid !important

```fscss
/* Bad */
.button { color: white !important; }

/* Better */
.sidebar .button { color: white; }
```

## Exercises

1. Create a complex card hover effect with nested selectors
2. Build a form with validation states using pseudo-classes
3. Design a table with alternating row colors

---

**Next: [Real World Examples](./real-world-examples.md)**
