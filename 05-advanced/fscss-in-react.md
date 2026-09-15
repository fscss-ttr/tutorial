# FSCSS in React

Use FSCSS in React projects. Write styles with FSCSS and apply them to React components.

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
    "start": "react-scripts start",
    "build": "react-scripts build",
    "fscss": "fscss src/styles.fscss src/index.css",
    "watch:fscss": "fscss --watch src/styles.fscss src/index.css"
  }
}
```

### 3. Run FSCSS

```bash
npm run fscss
```

## Basic Usage

### 1. Create FSCSS File

```fscss
/* src/styles.fscss */
$primary: #2563eb;
$spacing: 1rem;

.button {
  background: $primary;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  border: none;
  cursor: pointer;
}

.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: $spacing;
}
```

### 2. Compile to CSS

```bash
fscss src/styles.fscss src/index.css
```

### 3. Import in React

```jsx
// src/index.js
import './index.css';

function App() {
  return (
    <div className="card">
      <button className="button">Click Me</button>
    </div>
  );
}

export default App;
```

## Component-Based Styles

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
// src/components/Button.jsx
import '../components/Button.fscss';

function Button({ variant = 'primary', children }) {
  return (
    <button className={`button button-${variant}`}>
      {children}
    </button>
  );
}

export default Button;
```

## CSS Modules with FSCSS

### Setup CSS Modules

```javascript
// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.module\.css$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              modules: true,
            },
          },
        ],
      },
    ],
  },
};
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
// src/components/Card.jsx
import styles from './Card.module.fscss';

function Card({ title, children }) {
  return (
    <div className={styles.card}>
      <h2 className={styles.title}>{title}</h2>
      {children}
    </div>
  );
}

export default Card;
```

## Inline FSCSS

### Runtime Processing

```html
<!-- public/index.html -->
<style type="text/fscss">
  $primary: #2563eb;

  .dynamic {
    background: $primary;
    color: white;
  }
</style>
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.0/runtime.min.js" async></script>
```

## Watch Mode

### Development Workflow

```bash
# Terminal 1: Watch FSCSS
npm run watch:fscss

# Terminal 2: Start React
npm start
```

## Best Practices

### 1. Organize by Component

```
src/
├── components/
│   ├── Button/
│   │   ├── Button.jsx
│   │   └── Button.fscss
│   └── Card/
│       ├── Card.jsx
│       └── Card.fscss
└── styles/
    ├── variables.fscss
    └── global.fscss
```

### 2. Use Variables for Theming

```fscss
/* src/styles/variables.fscss */
$primary: #2563eb;
$spacing: 1rem;
$radius: 8px;
```

### 3. Keep Components Self-Contained

Each component manages its own styles.

## Exercises

1. Set up FSCSS in a React project
2. Create a button component with FSCSS
3. Build a card component with CSS Modules

---

**Next: [FSCSS in Vue](./fscss-in-vue.md)**
