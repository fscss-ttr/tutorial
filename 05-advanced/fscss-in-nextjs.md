# FSCSS in Next.js

Use FSCSS in Next.js projects. Server-side rendering compatible styling.

## Setup

### 1. Install FSCSS

```bash
npm install fscss@latest
```

### 2. Add Build Script

Update `package.json`:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "fscss": "fscss src/styles.fscss src/styles/globals.css",
    "watch:fscss": "fscss --watch src/styles.fscss src/styles/globals.css"
  }
}
```

## Basic Usage

### 1. Create Global Styles

```fscss
/* src/styles.fscss */
$primary: #2563eb;
$spacing: 1rem;

body {
  font-family: 'Inter', sans-serif;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: $spacing;
}
```

### 2. Import in Layout

```jsx
// src/app/layout.js
import '../styles/globals.css'

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

## Component Styles

### Create Component Styles

```fscss
/* src/components/Button.fscss */
$primary: #2563eb;
$danger: #dc2626;

.button {
  padding: 12px 24px;
  border-radius: 8px;
  border: none;
  font-weight: 600;
  cursor: pointer;
  -*transition: all 0.2s ease;
}

.button-primary {
  background: $primary;
  color: white;
}

.button-danger {
  background: $danger;
  color: white;
}
```

### Use in Component

```jsx
// src/components/Button.js
import '../components/Button.fscss'

export default function Button({ variant = 'primary', children }) {
  return (
    <button className={`button button-${variant}`}>
      {children}
    </button>
  )
}
```

## CSS Modules

### Setup CSS Modules

```javascript
// next.config.js
module.exports = {
  cssModules: true,
}
```

### Create Module Styles

```fscss
/* src/components/Card.module.fscss */
$shadow: 0 4px 6px rgba(0, 0, 0, 0.1);

.card {
  background: white;
  border-radius: 8px;
  box-shadow: $shadow;
  padding: 1.5rem;
}

.title {
  font-size: 1.25rem;
  font-weight: 600;
}
```

### Use in Component

```jsx
// src/components/Card.js
import styles from './Card.module.fscss'

export default function Card({ title, children }) {
  return (
    <div className={styles.card}>
      <h2 className={styles.title}>{title}</h2>
      {children}
    </div>
  )
}
```

## App Router Styling

### Layout with FSCSS

```jsx
// src/app/layout.js
import '../styles/globals.css'

export const metadata = {
  title: 'My App',
  description: 'Next.js with FSCSS',
}

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

### Page with FSCSS

```jsx
// src/app/page.js
import Button from '../components/Button'
import Card from '../components/Card'

export default function Home() {
  return (
    <div className="container">
      <Card title="Welcome">
        <p>Hello World</p>
        <Button>Click Me</Button>
      </Card>
    </div>
  )
}
```

## Best Practices

### 1. Global Styles First

```jsx
// src/app/layout.js
import '../styles/globals.css'
```

### 2. Component Styles

```jsx
// src/components/Button.js
import '../components/Button.fscss'
```

### 3. Organize by Feature

```
src/
├── app/
│   ├── layout.js
│   └── page.js
├── components/
│   ├── Button/
│   │   ├── Button.js
│   │   └── Button.fscss
│   └── Card/
│       ├── Card.js
│       └── Card.fscss
└── styles/
    ├── globals.fscss
    └── variables.fscss
```

## Exercises

1. Set up FSCSS in a Next.js project
2. Create a page with styled components
3. Build a layout with global FSCSS styles

---

**Next: [FSCSS in Python](./fscss-in-python.md)**
