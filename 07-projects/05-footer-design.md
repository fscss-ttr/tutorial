# 05 — Footer Design

Footers hold three kinds of content: branding, link columns, and legal legalese. Build all three from one link array and a couple of block defines, on a dark surface.

## The Brief

- A dark footer with a brand block, three link columns, and a bottom legal bar.
- All link columns generated from one array of `title → links`.
- A top border that separates it cleanly from the page.

## The FSCSS

```fscss
@fun(color){
  bg: #0b1220;
  surface: #111a2e;
  text: #94a3b8;
  strong: #e2e8f0;
}

@fun(space){ col: 1rem; }

@define footer-base() {
  `
  .footer {
    background: @fun.color.bg.value;
    color: @fun.color.text.value;
    padding: 4rem 2rem 2rem;
    margin-top: 4rem;
    border-top: 4px solid @fun.color.surface.value;
  }

  .footer-inner {
    display: grid;
    grid-template-columns: 1.5fr repeat(3, 1fr);
    gap: 2rem;
    max-width: 1100px;
    margin: 0 auto;
  }

  .footer-brand {
    color: @fun.color.strong.value;
    font-weight: 700;
    font-size: 1.15rem;
    margin-bottom: 0.75rem;
  }

  .footer-tagline {
    font-size: 0.9rem;
    line-height: 1.6;
    max-width: 280px;
  }

  .footer-col-title {
    color: @fun.color.strong.value;
    font-size: 0.85rem;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    margin-bottom: 0.75rem;
  }

  .footer-col a {
    display: block;
    color: @fun.color.text.value;
    text-decoration: none;
    padding: 0.25rem 0;
    font-size: 0.9rem;
  }

  .footer-col a:hover { color: @fun.color.strong.value; }

  .footer-legal {
    border-top: 1px solid @fun.color.surface.value;
    margin-top: 3rem;
    padding-top: 1.25rem;
    text-align: center;
    font-size: 0.82rem;
    color: #64748b;
  }

  @media (max-width: 900px) {
    .footer-inner { grid-template-columns: repeat(2, 1fr); gap: 1.5rem; }
  }
  `
}

@footer-base()
```

## The Columns (data)

```fscss
@arr(titles[Product, Company, Resources])
@arr(product[Features, Pricing, Changelog])
@arr(company[About, Careers, Contact])
@arr(resources[Docs, Blog, Support])

@arr(ti[count(3, 1)])
```

## The HTML

```html
<footer class="footer">
  <div class="footer-inner">
    <div>
      <div class="footer-brand">Brand</div>
      <p class="footer-tagline">Short, honest text about what this product does.</p>
    </div>
    <div class="footer-col">
      <div class="footer-col-title">Product</div>
      <a href="#">Features</a>
      <a href="#">Pricing</a>
      <a href="#">Changelog</a>
    </div>
    <div class="footer-col">
      <div class="footer-col-title">Company</div>
      <a href="#">About</a>
      <a href="#">Careers</a>
      <a href="#">Contact</a>
    </div>
    <div class="footer-col">
      <div class="footer-col-title">Resources</div>
      <a href="#">Docs</a>
      <a href="#">Blog</a>
      <a href="#">Support</a>
    </div>
  </div>
  <div class="footer-legal">© 2026 Brand. All rights reserved.</div>
</footer>
```

## What Happens

- `footer-base` is one block define holding the full structure — media query included.
- Link columns are plain HTML because FSCSS can't emit arbitrary HTML; the *styling* stays in the define.
- Tokens own the dark palette and the border.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Generate the columns from arrays into the CSS as styling only (colors/hover) — imagine the `footer-col` rules written once.
2. Add a newsletter input inside the brand column and style it.
3. Stack the columns into an accordion under 600px.

## Summary

One block define wrote the entire footer, including its breakpoint. Brand, three columns, legal bar — done.

---

**Next: [06 — Form Elements](../06-form-elements.md)**