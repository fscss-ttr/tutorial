# First Project: Personal Card

Build a personal card component using everything you've learned in the Beginner section.

## What You'll Build

A styled card component with:
- Profile image
- Name and title
- Bio text
- Social links
- Hover effects

## Step 1: Set Up Your Files

Create these files:

```
project/
├── index.html
└── style.fscss
```

## Step 2: Write the HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Personal Card</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="card">
    <img class="card-avatar" src="https://via.placeholder.com/150" alt="Profile">
    <h2 class="card-name">Jane Developer</h2>
    <p class="card-title">Frontend Developer</p>
    <p class="card-bio">Building beautiful web experiences with clean code and modern design.</p>
    <div class="card-links">
      <a href="#" class="card-link">GitHub</a>
      <a href="#" class="card-link">Twitter</a>
      <a href="#" class="card-link">LinkedIn</a>
    </div>
  </div>
</body>
</html>
```

## Step 3: Write the FSCSS

Open `style.fscss` and write:

```fscss
/* Variables */
$primary: #2563eb;
$dark: #1e293b;
$gray: #64748b;
$light: #f1f5f9;
$white: #ffffff;
$shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
$radius: 12px;

/* Reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', system-ui, sans-serif;
  background: $light;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
}

/* Card */
.card {
  background: $white;
  border-radius: $radius;
  box-shadow: $shadow;
  padding: 2rem;
  max-width: 320px;
  text-align: center;
  -*transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
  -*transform: translateY(-8px);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);
}

/* Avatar */
.card-avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 1rem;
  border: 4px solid $primary;
}

/* Name */
.card-name {
  font-size: 1.5rem;
  font-weight: 700;
  color: $dark;
  margin-bottom: 0.25rem;
}

/* Title */
.card-title {
  font-size: 0.875rem;
  color: $primary;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin-bottom: 1rem;
}

/* Bio */
.card-bio {
  font-size: 0.875rem;
  color: $gray;
  line-height: 1.6;
  margin-bottom: 1.5rem;
}

/* Links */
.card-links {
  display: flex;
  gap: 0.75rem;
  justify-content: center;
}

.card-link {
  display: inline-block;
  padding: 0.5rem 1rem;
  color: $primary;
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  border: 1px solid $primary;
  border-radius: 6px;
  -*transition: all 0.2s ease;
}

.card-link:hover {
  background: $primary;
  color: $white;
}
```

## Step 4: Compile and Test

```bash
fscss style.fscss style.css
```

Open `index.html` in your browser.

## Step 5: Enhance It

Try adding these features:

### Add More Variables

```fscss
$font-family: 'Inter', system-ui, sans-serif;
$font-size-sm: 0.875rem;
$font-size-base: 1rem;
$font-size-lg: 1.25rem;
$font-size-xl: 1.5rem;
```

### Create a Style Store

```fscss
@fun(card) {
  background: $white;
  border-radius: $radius;
  box-shadow: $shadow;
  padding: 2rem;
  max-width: 320px;
  text-align: center;
  -*transition: transform 0.3s ease, box-shadow 0.3s ease;
}
```

### Add Responsive Design

```fscss
@media (max-width: 480px) {
  .card {
    padding: 1.5rem;
  }

  .card-avatar {
    width: 80px;
    height: 80px;
  }

  .card-name {
    font-size: $font-size-lg;
  }
}
```

## What You Learned

1. **Variables** — Colors, shadows, border-radius
2. **Shorthand** — `-*transition` for vendor prefixes
3. **Style Stores** — Reusable card pattern
4. **Selectors** — Class selectors, hover states
5. **Organization** — Grouped related properties

## Next Steps

- Try changing the colors and sizes using variables
- Add more card variants (different colors, sizes)
- Create a grid of cards
- Move on to the [Intermediate](../04-intermediate/README.md) section

---

**Congratulations! You've completed the Beginner section. Continue to [Intermediate](../04-intermediate/README.md).**
