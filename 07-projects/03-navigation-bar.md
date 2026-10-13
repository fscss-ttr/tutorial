# 03 — Navigation Bar

A responsive nav: brand, links, a mobile menu toggle, a sticky top bar, and an active link state — all token-driven and generated from an array where it can be.

## The Brief

- Sticky header with brand, center links, and a CTA.
- Links generated from an array so adding one changes both nav and footer.
- Active link underline via `:is()` style targeting from the same data.
- Breakpoint at 768px collapses center links behind a toggle button.

## The FSCSS

```fscss
@fun(color){
  bg: #0f172a;
  text: #94a3b8;
  active: #ffffff;
  accent: #2563eb;
}

@arr(items[home, features, pricing, docs])
@arr(i[count(4, 1)])

.nav-link-@arr.items[@arr.i[]] {
  color: @fun.color.text.value;
  text-decoration: none;
  padding: 0.5rem 0.75rem;
  transition: color 0.15s ease;
}

.nav-link-@arr.items[@arr.i[]]:hover,
.nav-link-@arr.items[@arr.i[]].active {
  color: @fun.color.active.value;
}

.nav{
  position: sticky;
  top: 0;
  z-index: 50;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  padding: 0 1.5rem;
  background: @fun.color.bg.value;
  box-shadow: 0 1px 0 rgba(255,255,255,0.06);
}

.nav-brand {
  color: @fun.color.active.value;
  font-weight: 700;
  font-size: 1.15rem;
  text-decoration: none;
}

.nav-links {
  display: flex;
  gap: 0.25rem;
}

.nav-cta {
  border: none;
  border-radius: 8px;
  padding: 0.5rem 1rem;
  background: @fun.color.accent.value;
  color: #ffffff;
  cursor: pointer;
  font-weight: 600;
}

.nav-toggle {
  display: none;
  border: none;
  background: transparent;
  color: @fun.color.text.value;
  font-size: 1.5rem;
  cursor: pointer;
}
```

## The HTML

```html
<nav class="nav">
  <a class="nav-brand" href="#">Brand</a>
  <div class="nav-links">
    <a class="nav-link-home active" href="#">Home</a>
    <a class="nav-link-features" href="#">Features</a>
    <a class="nav-link-pricing" href="#">Pricing</a>
    <a class="nav-link-docs" href="#">Docs</a>
  </div>
  <button class="nav-cta">Get Started</button>
  <button class="nav-toggle">≡</button>
</nav>
```

## The Responsive Story (block define)

```fscss
@define mobile-nav() {
  `
  @media (max-width: 767px) {
    .nav-links {
      display: none;
      position: absolute;
      top: 100%;
      left: 0;
      right: 0;
      flex-direction: column;
      background: #0f172a;
      padding: 1rem;
    }
    .nav-toggle { display: block; }
    .nav-links.open { display: flex; }
  }
  `
}

@mobile-nav()
```

## What Happens

- The array generates all four link styles + hover/active pairs from one template — the class name contains the item name.
- One `.active` class on the current link powers the whole thing.
- The mobile behavior lives in a named block define, not prose scattered through a media query.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add the footer using the same `items` array.
2. Add a scroll-based shadow that appears after 40px of scroll (class toggle in JS).
3. Drive `.active` by comparing a page attribute with the item name in HTML.

## Summary

Data-driven links + a sticky, token-colored bar + a named mobile block define = a nav that stays tidy as it grows.

---

**Next: [04 — Hero Section](../04-hero-section.md)**