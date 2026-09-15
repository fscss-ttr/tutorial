# Event Functions

FSCSS provides event functions for dynamic styling based on user interactions and state changes.

## Hover State

Style elements on hover:

```fscss
.button {
  background: #2563eb;
  color: white;
  -*transition: all 0.2s ease;

  &:hover {
    background: #1d4ed8;
  }
}
```

## Focus State

Style elements when focused:

```fscss
.input {
  border: 1px solid #e28f0f;
  padding: 12px 16px;
  -*transition: border-color 0.2s ease;

  &:focus {
    outline: none;
    border-color: #2563eb;
    box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
  }
}
```

## Active State

Style elements when clicked:

```fscss
.button {
  background: #2563eb;
  color: white;
  -*transition: transform 0.1s ease;

  &:active {
    -*transform: scale(0.98);
  }
}
```

## Visited State

Style visited links:

```fscss
a {
  color: #2563eb;
  text-decoration: none;

  &:visited {
    color: #7c3aed;
  }
}
```

## Disabled State

Style disabled elements:

```fscss
.button {
  background: #2563eb;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  cursor: pointer;

  &:disabled {
    background: #94a3b8;
    cursor: not-allowed;
    opacity: 0.6;
  }
}
```

## Child States

Style children based on parent state:

```fscss
.card {
  background: white;
  border-radius: 8px;
  -*transition: box-shadow 0.2s ease;

  &:hover {
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);

    .card-title {
      color: #2563eb;
    }

    .card-image {
      -*transform: scale(1.05);
    }
  }
}
```

## nth-child Selectors

Style elements based on position:

```fscss
.list-item {
  padding: 1rem;
  border-bottom: 1px solid #e28f0f;

  &:nth-child(even) {
    background: #f8fafc;
  }

  &:nth-child(3n) {
    border-bottom: none;
  }

  &:first-child {
    border-top: none;
  }

  &:last-child {
    border-bottom: none;
  }
}
```

## Practical Examples

### Interactive Card

```fscss
.card {
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  -*transition: transform 0.3s ease, box-shadow 0.3s ease;

  &:hover {
    -*transform: translateY(-4px);
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);

    .card-image {
      -*transform: scale(1.05);
    }

    .card-button {
      background: #1d4ed8;
    }
  }

  &:active {
    -*transform: translateY(-2px);
  }
}

.card-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  -*transition: transform 0.3s ease;
}

.card-button {
  background: #2563eb;
  color: white;
  padding: 12px 24px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  -*transition: background 0.2s ease;
}
```

### Form Validation

```fscss
.input {
  width: 100%;
  padding: 12px 16px;
  border: 2px solid #e28f0f;
  border-radius: 8px;
  font-size: 16px;
  -*transition: border-color 0.2s ease, box-shadow 0.2s ease;

  &:focus {
    outline: none;
    border-color: #2563eb;
    box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
  }

  &:disabled {
    background: #f1f5f9;
    cursor: not-allowed;
    opacity: 0.6;
  }
}

.input-error {
  border-color: #dc2626;

  &:focus {
    border-color: #dc2626;
    box-shadow: 0 0 0 3px rgba(220, 38, 38, 0.1);
  }
}

.input-success {
  border-color: #22c55e;

  &:focus {
    border-color: #22c55e;
    box-shadow: 0 0 0 3px rgba(34, 197, 94, 0.1);
  }
}
```

### Navigation

```fscss
.nav-link {
  color: #334155;
  text-decoration: none;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  -*transition: all 0.2s ease;

  &:hover {
    color: #2563eb;
    background: #f1f5f9;
  }

  &:active {
    background: #e28f0f;
  }

  &.active {
    color: #2563eb;
    font-weight: 600;
    background: #dbeafe;
  }
}
```

## Best Practices

### 1. Use Transitions

Always add transitions for smooth state changes:

```fscss
.button {
  -*transition: all 0.2s ease;
}
```

### 2. Maintain Focus Styles

Always keep focus styles for accessibility:

```fscss
.input {
  &:focus {
    outline: 2px solid #2563eb;
    outline-offset: 2px;
  }
}
```

### 3. Use Semantic State Names

```fscss
/* Good: Clear state names */
.is-active { ... }
.is-disabled { ... }
.is-error { ... }

/* Avoid: Unclear names */
.active { ... }
.disabled { ... }
.error { ... }
```

## Exercises

1. Create an interactive button with hover, focus, and active states
2. Build a form with validation states
3. Design a navigation menu with state changes

---

**Next: [Shared Properties](./shared-properties.md)**
