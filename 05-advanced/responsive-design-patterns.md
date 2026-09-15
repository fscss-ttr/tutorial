# Responsive Design Patterns

Master responsive design with FSCSS. Build layouts that work on any screen size.

## Mobile-First Approach

### Base Styles (Mobile)

```fscss
.container {
  padding: 1rem;
}

.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}
```

### Tablet Breakpoint

```fscss
@media (min-width: 768px) {
  .container {
    padding: 2rem;
  }

  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
```

### Desktop Breakpoint

```fscss
@media (min-width: 1024px) {
  .container {
    padding: 2rem;
    max-width: 1200px;
    margin: 0 auto;
  }

  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

## Breakpoint Variables

```fscss
$breakpoint-sm: 640px;
$breakpoint-md: 768px;
$breakpoint-lg: 1024px;
$breakpoint-xl: 1280px;

.container {
  padding: 1rem;

  @media (min-width: $breakpoint-md) {
    padding: 2rem;
  }

  @media (min-width: $breakpoint-lg) {
    max-width: 1200px;
    margin: 0 auto;
  }
}
```

## Common Patterns

### Responsive Typography

```fscss
$h1-size: 2rem;
$h2-size: 1.5rem;

h1 {
  font-size: $h1-size;

  @media (min-width: 768px) {
    font-size: $h1-size * 1.25;
  }

  @media (min-width: 1024px) {
    font-size: $h1-size * 1.5;
  }
}
```

### Responsive Spacing

```fscss
$spacing-mobile: 1rem;
$spacing-tablet: 1.5rem;
$spacing-desktop: 2rem;

.section {
  padding: $spacing-mobile;

  @media (min-width: 768px) {
    padding: $spacing-tablet;
  }

  @media (min-width: 1024px) {
    padding: $spacing-desktop;
  }
}
```

### Responsive Images

```fscss
.image {
  width: 100%;
  height: auto;
  border-radius: 8px;
}

@media (min-width: 768px) {
  .image {
    max-width: 50%;
  }
}
```

## Layout Patterns

### Holy Grail Layout

```fscss
.layout {
  display: grid;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}

@media (min-width: 768px) {
  .layout {
    grid-template-columns: 250px 1fr 250px;
    grid-template-rows: auto 1fr auto;
  }
}
```

### Sidebar Layout

```fscss
.content-wrapper {
  display: flex;
  flex-direction: column;
}

@media (min-width: 768px) {
  .content-wrapper {
    flex-direction: row;
  }

  .sidebar {
    width: 250px;
    flex-shrink: 0;
  }

  .main {
    flex: 1;
  }
}
```

### Card Grid

```fscss
.card-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1.5rem;
}

@media (min-width: 640px) {
  .card-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .card-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

## Responsive Utilities

```fscss
/* Hide on mobile, show on desktop */
@media (min-width: 768px) {
  .hidden-mobile { display: block; }
}

/* Show on mobile, hide on desktop */
@media (min-width: 768px) {
  .visible-mobile { display: none; }
}
```

## Best Practices

### 1. Mobile-First

Start with mobile styles, add complexity for larger screens.

### 2. Use Relative Units

```fscss
/* Good */
.container {
  padding: 1rem;
  font-size: 16px;
}

/* Avoid */
.container {
  padding: 16px;
  font-size: 16px;
}
```

### 3. Test on Real Devices

Don't just resize the browser. Test on actual phones and tablets.

## Exercises

1. Build a responsive navigation menu
2. Create a responsive card grid
3. Design a mobile-first layout

---

**Next: [Animation Techniques](./animation-techniques.md)**
