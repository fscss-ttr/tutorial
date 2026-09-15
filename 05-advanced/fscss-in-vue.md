# FSCSS in Vue

Use FSCSS in Vue.js projects. Write component-scoped styles with FSCSS.

## Setup

### 1. Install FSCSS

```bash
npm install fscss@latest
```

### 2. Configure Build

Add to `package.json`:

```json
{
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "fscss": "fscss src/styles.fscss src/assets/styles.css",
    "watch:fscss": "fscss --watch src/styles.fscss src/assets/styles.css"
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

### 2. Import in Main

```javascript
// src/main.js
import { createApp } from 'vue'
import App from './App.vue'
import './assets/styles.css'

createApp(App).mount('#app')
```

## Component Styles

### Single File Components

```vue
<!-- src/components/Button.vue -->
<template>
  <button class="button" :class="variant">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    variant: {
      type: String,
      default: 'primary'
    }
  }
}
</script>

<style lang="scss" scoped>
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

.button.primary {
  background: $primary;
  color: white;
}

.button.danger {
  background: $danger;
  color: white;
}
</style>
```

### External FSCSS Files

```vue
<!-- src/components/Card.vue -->
<template>
  <div class="card">
    <h3 class="card-title">{{ title }}</h3>
    <div class="card-body">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import '../components/Card.fscss'

export default {
  name: 'Card',
  props: {
    title: String
  }
}
</script>
```

## Scoped Styles

### Using Scoped Attribute

```vue
<template>
  <div class="container">
    <p class="text">Scoped content</p>
  </div>
</template>

<style scoped>
.container {
  padding: 2rem;
}

.text {
  color: #334155;
}
</style>
```

## Global vs Scoped

### Global Styles

```vue
<style>
/* Applied globally */
.button {
  background: #2563eb;
}
</style>
```

### Scoped Styles

```vue
<style scoped>
/* Applied only to this component */
.button {
  background: #2563eb;
}
</style>
```

## Best Practices

### 1. Use Scoped Styles

```vue
<style scoped>
/* Component-specific styles */
</style>
```

### 2. Global Variables

```fscss
/* src/styles/variables.fscss */
$primary: #2563eb;
$spacing: 1rem;
```

### 3. Component Organization

```
src/
├── components/
│   ├── Button.vue
│   ├── Button.fscss
│   ├── Card.vue
│   └── Card.fscss
└── styles/
    ├── variables.fscss
    └── global.fscss
```

## Exercises

1. Set up FSCSS in a Vue project
2. Create a button component with scoped FSCSS
3. Build a card component with external FSCSS file

---

**Next: [FSCSS in Next.js](./fscss-in-nextjs.md)**
