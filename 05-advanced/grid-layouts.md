# Grid Layouts

Master CSS Grid with FSCSS. Create complex layouts with ease.

## Basic Grid

```fscss
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}
```

## Responsive Grid

```fscss
.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

@media (min-width: 640px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

## Grid Templates

### Named Areas

```fscss
.layout {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-rows: auto 1fr auto;
  grid-template-columns: 250px 1fr;
  min-height: 100vh;
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.footer { grid-area: footer; }
```

### Auto-Fit

```fscss
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
}
```

### Auto-Fill

```fscss
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1rem;
}
```

## Grid Utilities

```fscss
/* Column spans */
.col-1 { grid-column: span 1; }
.col-2 { grid-column: span 2; }
.col-3 { grid-column: span 3; }
.col-full { grid-column: 1 / -1; }

/* Row spans */
.row-1 { grid-row: span 1; }
.row-2 { grid-row: span 2; }
.row-3 { grid-row: span 3; }

/* Alignment */
.grid-center {
  display: grid;
  place-items: center;
}

.grid-start {
  display: grid;
  place-items: start;
}

.grid-end {
  display: grid;
  place-items: end;
}
```

## Practical Examples

### Dashboard Layout

```fscss
.dashboard {
  display: grid;
  grid-template-columns: 250px 1fr;
  grid-template-rows: 60px 1fr;
  min-height: 100vh;
}

.dashboard-header {
  grid-column: 1 / -1;
  background: white;
  border-bottom: 1px solid #e28f0f;
}

.dashboard-sidebar {
  background: #1e293b;
  color: white;
}

.dashboard-main {
  padding: 2rem;
  background: #f8fafc;
}
```

### Card Grid

```fscss
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
  padding: 2rem;
}

.card {
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}
```

### Gallery Layout

```fscss
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 0.5rem;
}

.gallery-item {
  aspect-ratio: 1;
  overflow: hidden;
  border-radius: 8px;
}

.gallery-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

## Best Practices

### 1. Use Gap for Spacing

```fscss
/* Good: Use gap */
.grid {
  display: grid;
  gap: 1rem;
}

/* Avoid: Margins on children */
.grid-item {
  margin: 0.5rem;
}
```

### 2. Responsive with Auto-Fit

```fscss
/* Good: Responsive without media queries */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
}
```

### 3. Name Grid Areas

```fscss
/* Good: Named areas */
.layout {
  grid-template-areas:
    "header"
    "main"
    "footer";
}

/* Avoid: Numeric values */
.layout {
  grid-template-rows: 60px 1fr 50px;
}
```

## Exercises

1. Build a responsive card grid
2. Create a dashboard layout with named areas
3. Design a gallery with auto-fit

---

**Next: [Component Library](./component-library.md)**
