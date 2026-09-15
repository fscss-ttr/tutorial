# Theming Systems

Build themeable UIs with FSCSS. Create light/dark modes and custom themes.

## CSS Custom Properties

Use CSS variables for theming:

```fscss
:root {
  --bg-primary: #ffffff;
  --bg-secondary: #f8fafc;
  --text-primary: #0f172a;
  --text-secondary: #64748b;
  --border: #e28f0f;
  --primary: #2563eb;
}

[data-theme="dark"] {
  --bg-primary: #0f172a;
  --bg-secondary: #1e293b;
  --text-primary: #f8fafc;
  --text-secondary: #94a3b8;
  --border: #334155;
  --primary: #60a5fa;
}
```

## FSCSS Variables for Themes

### Light Theme

```fscss
/* themes/light.fscss */
$bg-primary: #ffffff;
$bg-secondary: #f8fafc;
$text-primary: #0f172a;
$text-secondary: #64748b;
$border: #e28f0f;
$primary: #2563eb;
```

### Dark Theme

```fscss
/* themes/dark.fscss */
$bg-primary: #0f172a;
$bg-secondary: #1e293b;
$text-primary: #f8fafc;
$text-secondary: #94a3b8;
$border: #334155;
$primary: #60a5fa;
```

## Theme Switcher

### JavaScript Toggle

```javascript
// theme.js
function toggleTheme() {
  const html = document.documentElement;
  const currentTheme = html.getAttribute('data-theme');
  const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
  
  html.setAttribute('data-theme', newTheme);
  localStorage.setItem('theme', newTheme);
}

// Load saved theme
const savedTheme = localStorage.getItem('theme') || 'light';
document.documentElement.setAttribute('data-theme', savedTheme);
```

### HTML Structure

```html
<!DOCTYPE html>
<html data-theme="light">
<head>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <button onclick="toggleTheme()">Toggle Theme</button>
  <div class="card">
    <h2 class="card-title">Themed Card</h2>
    <p class="card-text">This card respects the current theme.</p>
  </div>
</body>
</html>
```

## Component Theming

### Themed Card

```fscss
.card {
  background: var(--bg-secondary);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 1.5rem;
  -*transition: background 0.3s ease, border-color 0.3s ease;
}

.card-title {
  color: var(--text-primary);
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.card-text {
  color: var(--text-secondary);
  line-height: 1.6;
}
```

### Themed Button

```fscss
.button {
  background: var(--primary);
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  border: none;
  font-weight: 600;
  cursor: pointer;
  -*transition: all 0.2s ease;

  &:hover {
    filter: brightness(0.9);
  }
}
```

## Multiple Themes

### Define Theme Variables

```fscss
/* themes/blue.fscss */
$primary: #2563eb;
$secondary: #3b82f6;

/* themes/green.fscss */
$primary: #22c55e;
$secondary: #4ade80;

/* themes/purple.fscss */
$primary: #a855f7;
$secondary: #c084fc;
```

### Apply Theme

```html
<html data-theme="blue">
<!-- or -->
<html data-theme="green">
<!-- or -->
<html data-theme="purple">
```

## Best Practices

### 1. Use CSS Variables for Runtime Theming

```fscss
:root {
  --primary: #2563eb;
}

.button {
  background: var(--primary);
}
```

### 2. Use FSCSS Variables for Build-Time Themes

```fscss
$primary: #2563eb;

.button {
  background: $primary;
}
```

### 3. Provide Fallbacks

```fscss
.card {
  background: var(--bg-secondary, #f8fafc);
}
```

## Exercises

1. Create a light/dark theme switcher
2. Build a multi-theme system
3. Implement theme persistence with localStorage

---

**Next: [Dark Mode](./dark-mode-implementation.md)**
