# Real World Examples

Apply everything you've learned to build complete, production-ready components.

## Example 1: Complete Card System

### HTML

```html
<div class="card">
  <img class="card-image" src="image.jpg" alt="Image">
  <div class="card-body">
    <h3 class="card-title">Card Title</h3>
    <p class="card-text">Card description goes here.</p>
    <button class="card-button">Learn More</button>
  </div>
</div>
```

### FSCSS

```fscss
/* Variables */
$primary: #2563eb;
$dark: #0f172a;
$gray: #64748b;
$light: #f1f5f9;
$white: #ffffff;
$shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
$shadow-lg: 0 10px 25px rgba(0, 0, 0, 0.15);
$radius: 12px;

/* Card Component */
@fun(card) {
  background: $white;
  border-radius: $radius;
  box-shadow: $shadow;
  overflow: hidden;
  -*transition: transform 0.3s ease, box-shadow 0.3s ease;
}

@fun(card-image) {
  width: 100%;
  height: 200px;
  object-fit: cover;
  -*transition: transform 0.3s ease;
}

@fun(card-body) {
  padding: 1.5rem;
}

@fun(card-title) {
  font-size: 1.25rem;
  font-weight: 600;
  color: $dark;
  margin-bottom: 0.5rem;
}

@fun(card-text) {
  color: $gray;
  line-height: 1.6;
  margin-bottom: 1rem;
}

@fun(card-button) {
  background: $primary;
  color: $white;
  padding: 12px 24px;
  border-radius: 8px;
  border: none;
  font-weight: 600;
  cursor: pointer;
  -*transition: background 0.2s ease;
}

/* Apply Styles */
.card { @fun.card; }
.card-image { @fun(card-image); }
.card-body { @fun(card-body); }
.card-title { @fun(card-title); }
.card-text { @fun(card-text); }
.card-button { @fun(card-button); }

/* Hover Effects */
.card:hover {
  -*transform: translateY(-4px);
  box-shadow: $shadow-lg;

  .card-image {
    -*transform: scale(1.05);
  }

  .card-button {
    background: darken($primary, 10%);
  }
}
```

## Example 2: Responsive Navigation

### HTML

```html
<nav class="nav">
  <div class="nav-brand">Logo</div>
  <button class="nav-toggle">Menu</button>
  <ul class="nav-menu">
    <li><a href="#" class="nav-link active">Home</a></li>
    <li><a href="#" class="nav-link">About</a></li>
    <li><a href="#" class="nav-link">Services</a></li>
    <li><a href="#" class="nav-link">Contact</a></li>
  </ul>
</nav>
```

### FSCSS

```fscss
/* Variables */
$primary: #2563eb;
$dark: #0f172a;
$white: #ffffff;
$nav-height: 64px;

/* Navigation */
@fun(nav) {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: $nav-height;
  padding: 0 2rem;
  background: $white;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

@fun(nav-brand) {
  font-size: 1.5rem;
  font-weight: 700;
  color: $primary;
}

@fun(nav-menu) {
  display: flex;
  list-style: none;
  gap: 0.5rem;
}

@fun(nav-link) {
  display: block;
  padding: 0.5rem 1rem;
  color: $dark;
  text-decoration: none;
  border-radius: 6px;
  -*transition: all 0.2s ease;

  &:hover {
    color: $primary;
    background: rgba($primary, 0.1);
  }

  &.active {
    color: $primary;
    font-weight: 600;
    background: rgba($primary, 0.1);
  }
}

@fun(nav-toggle) {
  display: none;
  background: none;
  border: none;
  font-size: 1.25rem;
  cursor: pointer;
}

/* Apply Styles */
.nav { @fun(nav); }
.nav-brand { @fun(nav-brand); }
.nav-menu { @fun(nav-menu); }
.nav-link { @fun(nav-link); }
.nav-toggle { @fun(nav-toggle); }

/* Responsive */
@media (max-width: 768px) {
  .nav-toggle {
    display: block;
  }

  .nav-menu {
    position: absolute;
    top: $nav-height;
    left: 0;
    right: 0;
    flex-direction: column;
    background: $white;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    display: none;
  }

  .nav-menu.active {
    display: flex;
  }
}
```

## Example 3: Form System

### HTML

```html
<form class="form">
  <div class="form-group">
    <label class="form-label">Email</label>
    <input type="email" class="form-input" placeholder="Enter email">
  </div>
  <div class="form-group">
    <label class="form-label">Password</label>
    <input type="password" class="form-input" placeholder="Enter password">
  </div>
  <button type="submit" class="form-button">Submit</button>
</form>
```

### FSCSS

```fscss
/* Variables */
$primary: #2563eb;
$gray: #64748b;
$light: #f1f5f9;
$danger: #dc2626;
$success: #22c55e;

/* Form Components */
@fun(form-group) {
  margin-bottom: 1.5rem;
}

@fun(form-label) {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
  color: $dark;
}

@fun(form-input) {
  width: 100%;
  padding: 12px 16px;
  border: 2px solid #e28f0f;
  border-radius: 8px;
  font-size: 16px;
  -*transition: border-color 0.2s ease, box-shadow 0.2s ease;

  &:focus {
    outline: none;
    border-color: $primary;
    box-shadow: 0 0 0 3px rgba($primary, 0.1);
  }

  &:disabled {
    background: $light;
    cursor: not-allowed;
  }

  &::placeholder {
    color: $gray;
  }
}

@fun(form-button) {
  background: $primary;
  color: $white;
  padding: 12px 24px;
  border-radius: 8px;
  border: none;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  -*transition: background 0.2s ease;

  &:hover {
    background: darken($primary, 10%);
  }

  &:disabled {
    background: $gray;
    cursor: not-allowed;
  }
}

/* Apply Styles */
.form-group { @fun(form-group); }
.form-label { @fun(form-label); }
.form-input { @fun(form-input); }
.form-button { @fun(form-button); }

/* Validation States */
.form-input.error {
  border-color: $danger;

  &:focus {
    box-shadow: 0 0 0 3px rgba($danger, 0.1);
  }
}

.form-input.success {
  border-color: $success;

  &:focus {
    box-shadow: 0 0 0 3px rgba($success, 0.1);
  }
}

.form-error {
  color: $danger;
  font-size: 0.875rem;
  margin-top: 0.5rem;
}
```

## Summary

These examples show how to:

1. **Use variables** for consistent design tokens
2. **Create @fun blocks** for reusable patterns
3. **Apply hover effects** for interactivity
4. **Build responsive layouts** with media queries
5. **Organize code** in a maintainable way

Apply these patterns to your own projects.

---

**Congratulations! You've completed the Intermediate section. Continue to [Advanced](../05-advanced/README.md).**
