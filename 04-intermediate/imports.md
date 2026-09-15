# Imports

Split your FSCSS into multiple files and combine them with imports. Keep your code organized and maintainable.

## Basic Import

Import another FSCSS file:

```fscss
@import 'variables';
```

This imports the contents of `variables.fscss` into the current file.

## Import Paths

### Same Directory

```fscss
@import 'variables';
@import 'mixins';
@import 'base';
```

### Subdirectory

```fscss
@import 'components/card';
@import 'components/button';
@import 'layout/header';
```

### Parent Directory

```fscss
@import '../variables';
@import '../mixins';
```

## Import Organization

### Recommended Structure

```fscss
/* main.fscss */

/* 1. Variables first */
@import 'variables';

/* 2. Reset/Base */
@import 'reset';
@import 'typography';

/* 3. Components */
@import 'components/card';
@import 'components/button';
@import 'components/input';

/* 4. Layout */
@import 'layout/header';
@import 'layout/footer';

/* 5. Utilities */
@import 'utilities/spacing';
@import 'utilities/colors';
```

### Why Order Matters

Variables must be imported before they're used:

```fscss
/* variables.fscss */
$primary: #2563eb;

/* button.fscss - uses $primary */
.btn {
  background: $primary;
}

/* main.fscss */
@import 'variables';  /* First! */
@import 'button';     /* Uses $primary */
```

## Importing Components

### Single Component

```fscss
/* Import just the card component */
@import 'components/card';
```

### Multiple Components

```fscss
/* Import all components */
@import 'components/card';
@import 'components/button';
@import 'components/input';
@import 'components/modal';
@import 'components/alert';
```

### Using Wildcards

Some FSCSS setups support wildcard imports:

```fscss
/* Import everything from components folder */
@import 'components/*';
```

## Import with Variables

Pass variables to imported files:

```fscss
/* variables.fscss */
$primary: #2563eb;
$spacing: 1rem;

/* components/card.fscss */
.card {
  background: white;
  padding: $spacing;
  border-radius: 8px;
}

/* main.fscss */
@import 'variables';
@import 'components/card';
```

## Modular Imports

### Component-Based

```fscss
/* main.fscss */
@import 'base/variables';
@import 'base/reset';

@import 'components/card';
@import 'components/button';
@import 'components/form';

@import 'layout/grid';
@import 'layout/header';
@import 'layout/footer';

@import 'utils/spacing';
@import 'utils/typography';
```

### Feature-Based

```fscss
/* For a blog */
@import 'blog/variables';
@import 'blog/posts';
@import 'blog/comments';
@import 'blog/sidebar';

/* For an e-commerce site */
@import 'shop/products';
@import 'shop/cart';
@import 'shop/checkout';
```

## Import Best Practices

### 1. One File, One Purpose

```fscss
/* Good: card.fscss handles only card styles */
.card { ... }
.card-header { ... }
.card-body { ... }

/* Bad: components.fscss handles everything */
.card { ... }
.btn { ... }
.input { ... }
.modal { ... }
```

### 2. Consistent Naming

```
components/
├── card.fscss
├── button.fscss
├── input.fscss
└── modal.fscss
```

Not:

```
components/
├── Card.fscss
├── button-component.fscss
├── input-field.fscss
└── modal-box-v2.fscss
```

### 3. Avoid Deep Nesting

```fscss
/* Good */
@import 'components/card';

/* Avoid */
@import 'src/styles/components/card';
```

### 4. Use Meaningful Names

```fscss
/* Good */
@import 'components/alert';
@import 'layout/header';

/* Avoid */
@import 'comp1';
@import 'h';
```

## Troubleshooting

### Variable Not Found

```fscss
/* Error: $primary is undefined */
.btn {
  background: $primary;
}
```

**Solution:** Import variables before using them:

```fscss
@import 'variables';  /* Add this */
.btn {
  background: $primary;
}
```

### Circular Import

```fscss
/* a.fscss */
@import 'b';

/* b.fscss */
@import 'a';  /* Circular! */
```

**Solution:** Restructure to avoid circular dependencies.

### File Not Found

```fscss
@import 'components/card';  /* Error: file not found */
```

**Solution:** Check the file path and name:

```bash
ls -la components/
```

## Exercises

1. Create a modular FSCSS structure with imports
2. Split a large stylesheet into logical modules
3. Import components in the correct order

---

**Next: [External Modules](./external-modules.md)**
