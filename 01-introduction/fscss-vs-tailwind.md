# Why FSCSS Over Other Tools

FSCSS isn't just another option — it's the better choice for developers who want speed, simplicity, and control.

## The Problem with Utility-First Frameworks

Utility-first frameworks force you to write styles in your HTML. This creates several issues:

### 1. Messy HTML

```html
<div class="flex items-center justify-between bg-white rounded-lg shadow-md p-4 mb-6 border border-gray-200 hover:shadow-lg transition-shadow duration-300">
  <div class="flex items-center space-x-4">
    <img class="w-12 h-12 rounded-full object-cover" src="avatar.jpg" />
    <div>
      <h3 class="text-lg font-semibold text-gray-900">User Name</h3>
      <p class="text-sm text-gray-500">Software Developer</p>
    </div>
  </div>
</div>
```

This is unreadable and unmaintainable.

### 2. Memorizing Hundreds of Classes

You need to remember:
- `bg-blue-600` vs `bg-blue-500` vs `bg-blue-700`
- `p-4` vs `px-4` vs `py-4` vs `pt-4`
- `rounded-lg` vs `rounded-md` vs `rounded-full`
- `shadow-sm` vs `shadow-md` vs `shadow-lg`

This is mental overhead you don't need.

### 3. No Semantic Naming

Your HTML becomes a wall of utility classes with no meaningful structure. Try explaining `div class="flex items-center space-x-3 p-2 bg-gray-100 rounded"` to a teammate.

## How FSCSS Solves This

### Clean, Semantic HTML

```html
<div class="user-card">
  <img class="user-avatar" src="avatar.jpg" />
  <div class="user-info">
    <h3 class="user-name">User Name</h3>
    <p class="user-role">Software Developer</p>
  </div>
</div>
```

### Styles in Your Stylesheet

```fscss
.user-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1rem;
  margin-bottom: 1.5rem;
}

.user-avatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  object-fit: cover;
}

.user-name {
  font-size: 1.125rem;
  font-weight: 600;
  color: #111827;
}
```

### Reusable Components with @define

```fscss
@define card(shadow, radius) {
  background: white;
  border-radius: @use(radius);
  box-shadow: @use(shadow);
  padding: 1.5rem;
}

.user-card { @card(0 4px 6px #121212, 8px) }
.blog-card { @card(0 2px 4px #999888, 12px) }
.product-card { @card(0 10px 10px #345434, 16px) }
```

## Comparison Table

| Feature | FSCSS | Utility-First Frameworks |
|---------|-------|--------------------------|
| HTML cleanliness | Clean, semantic | Cluttered with classes |
| Learning curve | CSS + shorthand | Hundreds of class names |
| Naming control | Full freedom | Locked to framework |
| Code organization | Stylesheets | Mixed in HTML |
| Readability | High | Low |
| Maintenance | Easy | Difficult |
| Bundle size | Only what you write | Framework CSS |
| Customization | Unlimited | Theme config only |

## Real Developer Experience

### With FSCSS

1. Write meaningful class names in HTML
2. Define styles in your stylesheet using shorthand
3. Reuse patterns with @define and @fun
4. Ship clean, maintainable code

### With Utility-First

1. Write long strings of utility classes in HTML
2. Remember hundreds of class combinations
3. Debug which class does what
4. Ship bloated, hard-to-read HTML

## The FSCSS Advantage

### 1. Write CSS 50% Faster

Shorthand syntax means fewer keystrokes:

```fscss
/* FSCSS shorthand */
-*-transform: rotate(45deg);

/* Compiles to standard CSS */
-webkit-transform: rotate(45deg);
-moz-transform: rotate(45deg);
-ms-transform: rotate(45deg);
-o-transform: rotate(45deg);
transform: rotate(45deg);
```

### 2. Variables That Actually Work

```fscss
$primary: #2563eb;
$spacing: 1rem;
$radius: 8px;

.button {
  background: $primary;
  padding: $spacing;
  border-radius: $radius;
}
```

### 3. Style Stores for Patterns

```fscss
@fun(card) {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
}

.product-card { @fun.card }
.blog-card { @fun.card }
```


## Summary

FSCSS gives you the power of a preprocessor with the simplicity of shorthand syntax. You write clean HTML, organized stylesheets, and maintainable code. Utility-first frameworks give you cluttered HTML, memorized class names, and locked-in conventions.

Choose FSCSS. Write better CSS, faster.

---

**Next: [When to Use FSCSS](./when-to-use-fscss.md)**
