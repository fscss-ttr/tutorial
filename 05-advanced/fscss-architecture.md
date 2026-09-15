# FSCSS Architecture

Structure large FSCSS projects for maintainability and scalability.

## Architecture Principles

### 1. Single Responsibility

Each file has one purpose:

```fscss
/* Good: One component per file */
/* components/button.fscss */
.btn { ... }
.btn-primary { ... }

/* Bad: Multiple unrelated styles */
/* styles.fscss */
.btn { ... }
.card { ... }
.header { ... }
.footer { ... }
```

### 2. Separation of Concerns

Separate different types of code:

```
src/styles/
├── base/           # Reset, typography, base elements
├── components/     # Reusable UI components
├── layout/         # Page layouts, grids
├── utilities/      # Helper classes
└── vendors/        # Third-party styles
```

### 3. Don't Repeat Yourself (DRY)

Use variables, @fun, and @define to avoid repetition:

```fscss
/* Bad: Repeated values */
.card-1 { background: #ffffff; border-radius: 8px; }
.card-2 { background: #ffffff; border-radius: 8px; }
.card-3 { background: #ffffff; border-radius: 8px; }

/* Good: Reusable pattern */
@fun(card) {
  background: #ffffff;
  border-radius: 8px;
}

.card-1 { @fun(card); }
.card-2 { @fun(card); }
.card-3 { @fun(card); }
```

## Project Structure

### Small Projects

```
project/
├── styles/
│   ├── variables.fscss
│   ├── components.fscss
│   ├── layout.fscss
│   └── main.fscss
└── index.html
```

### Medium Projects

```
project/
├── src/
│   └── styles/
│       ├── base/
│       │   ├── variables.fscss
│       │   ├── reset.fscss
│       │   └── typography.fscss
│       ├── components/
│       │   ├── button.fscss
│       │   ├── card.fscss
│       │   └── input.fscss
│       ├── layout/
│       │   ├── header.fscss
│       │   ├── footer.fscss
│       │   └── grid.fscss
│       ├── utilities/
│       │   ├── spacing.fscss
│       │   └── display.fscss
│       └── main.fscss
└── package.json
```

### Large Projects

```
project/
├── src/
│   └── styles/
│       ├── abstracts/         # Variables, mixins, functions
│       │   ├── variables/
│       │   ├── mixins/
│       │   └── functions/
│       ├── base/              # Reset, typography, elements
│       │   ├── reset.fscss
│       │   ├── typography.fscss
│       │   └── elements.fscss
│       ├── components/        # Reusable components
│       │   ├── button/
│       │   ├── card/
│       │   └── input/
│       ├── layout/            # Layout components
│       │   ├── header/
│       │   ├── footer/
│       │   └── grid/
│       ├── pages/             # Page-specific styles
│       │   ├── home.fscss
│       │   ├── about.fscss
│       │   └── contact.fscss
│       ├── utilities/         # Helper classes
│       │   ├── spacing.fscss
│       │   └── display.fscss
│       └── vendors/           # Third-party styles
│           └── bootstrap.fscss
└── package.json
```

## Naming Conventions

### Files

```fscss
/* Use lowercase with hyphens */
button.fscss
card-header.fscss
form-input.fscss

/* Avoid */
Button.fscss
card_header.fscss
FormInput.fscss
```

### Classes

```fscss
/* BEM naming */
.card { ... }
.card__header { ... }
.card__body { ... }
.card--primary { ... }

/* Utility classes */
.flex-center { ... }
.text-bold { ... }
.mt-4 { ... }
```

### Variables

```fscss
/* Use kebab-case */
$primary-color: #2563eb;
$spacing-md: 1rem;
$font-size-base: 16px;

/* Avoid */
$primaryColor: #2563eb;
$spacing_md: 1rem;
$fontsizebase: 16px;
```

## Import Strategy

### Load Order

```fscss
/* main.fscss */

/* 1. Abstracts (variables, mixins) */
@import 'abstracts/variables';
@import 'abstracts/mixins';

/* 2. Base (reset, typography) */
@import 'base/reset';
@import 'base/typography';

/* 3. Components */
@import 'components/button';
@import 'components/card';
@import 'components/input';

/* 4. Layout */
@import 'layout/header';
@import 'layout/footer';

/* 5. Utilities */
@import 'utilities/spacing';
@import 'utilities/display';
```

## Best Practices

### 1. Use Partials

Prefix files with underscore to indicate they're meant to be imported:

```
_abstracts/
├── _variables.fscss
├── _mixins.fscss
└── _functions.fscss
```

### 2. Avoid !important

```fscss
/* Bad */
.button { color: white !important; }

/* Better */
.sidebar .button { color: white; }
```

### 3. Keep Specificity Low

```fscss
/* Good */
.card { ... }
.card-title { ... }

/* Bad */
#main .content .card .header .title { ... }
```

## Exercises

1. Set up a medium-sized project structure
2. Create a naming convention guide
3. Build a component library with proper architecture

---

**Next: [Performance Optimization](./performance-optimization.md)**
