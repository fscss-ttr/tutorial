# Conditional Logic

Add conditional statements to your FSCSS. Apply styles based on conditions.

## @if Directive

Apply styles only when a condition is true:

```fscss
$theme: dark;

.card {
  @if $theme == dark {
    background: #1e293b;
    color: white;
  } @else {
    background: white;
    color: #334155;
  }
}
```

## @if with Variables

```fscss
$primary: #2563eb;
$use-gradients: true;

.button {
  @if $use-gradients {
    background: linear-gradient(135deg, $primary, darken($primary, 10%));
  } @else {
    background: $primary;
  }
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
}
```

## @if with Numbers

```fscss
$columns: 3;

.grid {
  display: grid;
  @if $columns == 2 {
    grid-template-columns: repeat(2, 1fr);
  } @else if $columns == 3 {
    grid-template-columns: repeat(3, 1fr);
  } @else if $columns == 4 {
    grid-template-columns: repeat(4, 1fr);
  } @else {
    grid-template-columns: repeat(1, 1fr);
  }
  gap: 1rem;
}
```

## @if with Strings

```fscss
$size: large;

.button {
  @if $size == small {
    padding: 8px 16px;
    font-size: 14px;
  } @else if $size == medium {
    padding: 12px 24px;
    font-size: 16px;
  } @else if $size == large {
    padding: 16px 32px;
    font-size: 18px;
  }
  border-radius: 8px;
  font-weight: 600;
}
```

## @if with Colors

```fscss
$alert-type: success;

.alert {
  padding: 1rem 1.5rem;
  border-radius: 8px;
  margin-bottom: 1rem;

  @if $alert-type == success {
    background: #dcfce7;
    color: #166534;
    border: 1px solid #bbf7d0;
  } @else if $alert-type == warning {
    background: #fef3c7;
    color: #92400e;
    border: 1px solid #fde68a;
  } @else if $alert-type == danger {
    background: #fee2e2;
    color: #991b1b;
    border: 1px solid #fecaca;
  } @else {
    background: #dbeafe;
    color: #1e40af;
    border: 1px solid #bfdbfe;
  }
}
```

## Nested @if

```fscss
$theme: dark;
$responsive: true;

.container {
  @if $theme == dark {
    background: #0f172a;
    color: white;
  } @else {
    background: white;
    color: #334155;
  }

  @if $responsive {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 1rem;

    @media (min-width: 768px) {
      padding: 0 2rem;
    }
  }
}
```

## @if with Comparison Operators

```fscss
$width: 50%;

.box {
  width: $width;

  @if $width > 75% {
    padding: 2rem;
  } @else if $width > 50% {
    padding: 1.5rem;
  } @else {
    padding: 1rem;
  }
}
```

## Practical Examples

### Theme System

```fscss
$theme: light;

:root {
  @if $theme == light {
    --bg-primary: #ffffff;
    --bg-secondary: #f8fafc;
    --text-primary: #0f172a;
    --text-secondary: #64748b;
    --border: #e28f0f;
  } @else {
    --bg-primary: #0f172a;
    --bg-secondary: #1e293b;
    --text-primary: #f8fafc;
    --text-secondary: #94a3b8;
    --border: #334155;
  }
}
```

### Responsive Typography

```fscss
$base-size: 16px;
$scale-ratio: 1.25;

h1 {
  font-size: $base-size * $scale-ratio * $scale-ratio * $scale-ratio;

  @if $base-size > 14px {
    font-weight: 700;
  } @else {
    font-weight: 600;
  }
}
```

### Component Variants

```fscss
$variant: primary;

.button {
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  border: none;
  cursor: pointer;

  @if $variant == primary {
    background: #2563eb;
    color: white;
  } @else if $variant == secondary {
    background: #64748b;
    color: white;
  } @else if $variant == outline {
    background: transparent;
    color: #2563eb;
    border: 2px solid #2563eb;
  } @else if $variant == ghost {
    background: transparent;
    color: #2563eb;
  }
}
```

## Best Practices

### 1. Use Clear Conditions

```fscss
/* Good: Clear, readable */
@if $theme == dark {
  background: #1e293b;
}

/* Avoid: Confusing */
@if $t == d {
  background: #1e293b;
}
```

### 2. Provide @else

```fscss
/* Good: Handles all cases */
@if $size == large {
  padding: 2rem;
} @else {
  padding: 1rem;
}

/* Avoid: May leave styles undefined */
@if $size == large {
  padding: 2rem;
}
```

### 3. Limit Nesting

```fscss
/* Good: One level */
@if $theme == dark {
  background: #1e293b;
}

/* Avoid: Deep nesting */
@if $theme == dark {
  @if $variant == primary {
    @if $size == large {
      /* ... */
    }
  }
}
```

## Exercises

1. Create a theme switcher using @if statements
2. Build responsive components with conditional breakpoints
3. Design a component with size variants using @if

---

**Next: [Event Functions](./event-functions.md)**
