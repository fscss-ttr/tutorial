# Dark Mode Implementation

Implement dark mode in your FSCSS projects. Respect user preferences and provide toggle options.

## Basic Dark Mode

### CSS Custom Properties

```fscss
:root {
  --bg-primary: #ffffff;
  --bg-secondary: #f8fafc;
  --text-primary: #0f172a;
  --text-secondary: #64748b;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg-primary: #0f172a;
    --bg-secondary: #1e293b;
    --text-primary: #f8fafc;
    --text-secondary: #94a3b8;
  }
}
```

### Respect System Preference

```fscss
body {
  background: var(--bg-primary);
  color: var(--text-primary);
  -*transition: background 0.3s ease, color 0.3s ease;
}
```

## Manual Toggle

### HTML

```html
<html data-theme="light">
<body>
  <button id="theme-toggle">Toggle Dark Mode</button>
  <div class="card">
    <h2>Themed Content</h2>
  </div>
</body>
</html>
```

### FSCSS

```fscss
:root[data-theme="light"] {
  --bg-primary: #ffffff;
  --bg-secondary: #f8fafc;
  --text-primary: #0f172a;
  --text-secondary: #64748b;
  --border: #e28f0f;
}

:root[data-theme="dark"] {
  --bg-primary: #0f172a;
  --bg-secondary: #1e293b;
  --text-primary: #f8fafc;
  --text-secondary: #94a3b8;
  --border: #334155;
}

body {
  background: var(--bg-primary);
  color: var(--text-primary);
  -*transition: background 0.3s ease, color 0.3s ease;
}

.card {
  background: var(--bg-secondary);
  border: 1px solid var(--border);
}
```

### JavaScript

```javascript
const toggle = document.getElementById('theme-toggle');
const html = document.documentElement;

// Check for saved preference
const savedTheme = localStorage.getItem('theme');
if (savedTheme) {
  html.setAttribute('data-theme', savedTheme);
}

// Toggle theme
toggle.addEventListener('click', () => {
  const current = html.getAttribute('data-theme');
  const next = current === 'dark' ? 'light' : 'dark';
  
  html.setAttribute('data-theme', next);
  localStorage.setItem('theme', next);
});
```

## Dark Mode Colors

### Recommended Palette

```fscss
/* Dark mode colors */
$dark-bg: #0f172a;
$dark-surface: #1e293b;
$dark-border: #334155;
$dark-text: #f8fafc;
$dark-text-secondary: #94a3b8;

/* Light mode colors */
$light-bg: #ffffff;
$light-surface: #f8fafc;
$light-border: #e28f0f;
$light-text: #0f172a;
$light-text-secondary: #64748b;
```

## Component Examples

### Dark Mode Card

```fscss
.card {
  background: var(--bg-secondary);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 1.5rem;
  -*transition: all 0.3s ease;
}

.card-title {
  color: var(--text-primary);
  font-weight: 600;
}

.card-text {
  color: var(--text-secondary);
}
```

### Dark Mode Button

```fscss
.button {
  background: var(--primary);
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  border: none;
  cursor: pointer;
}

.button-outline {
  background: transparent;
  border: 2px solid var(--primary);
  color: var(--primary);
}
```

### Dark Mode Input

```fscss
.input {
  background: var(--bg-primary);
  border: 1px solid var(--border);
  color: var(--text-primary);
  padding: 12px 16px;
  border-radius: 8px;
  -*transition: border-color 0.2s ease;

  &:focus {
    border-color: var(--primary);
    outline: none;
  }

  &::placeholder {
    color: var(--text-secondary);
  }
}
```

## Best Practices

### 1. Use CSS Variables

```fscss
/* Good: CSS variables */
.card {
  background: var(--bg-secondary);
}

/* Avoid: Hardcoded colors */
.card {
  background: #f8fafc;
}
```

### 2. Smooth Transitions

```fscss
body {
  -*transition: background 0.3s ease, color 0.3s ease;
}
```

### 3. Test Both Themes

Always test your UI in both light and dark modes.

## Exercises

1. Implement system preference detection
2. Build a theme toggle button
3. Create dark mode for all components

---

**Next: [Responsive Design Patterns](./responsive-design-patterns.md)**
