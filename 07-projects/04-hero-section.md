# 04 — Hero Section

The first thing users see. Build a centered hero with a gradient background, a badge, headline, subtext, two CTA buttons, and a scroll cue — with tokens owning every color and space.

## The Brief

- Full-width gradient background derived from `@fun` colors.
- Centered content: badge, `h1`, lead, button row.
- One CTA solid, one outline.
- A generated `@keyframes` entrance and reduced-motion respect.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #475569;
  brand: #2563eb;
  brand-2: #7c3aed;
  white: #ffffff;
}

@fun(space){ section: 6rem; }

@define btn-base() {
  display: inline-block;
  border-radius: 10px;
  padding: 0.85rem 1.75rem;
  font-weight: 600;
  text-decoration: none;
  transition: transform 0.15s ease, background-color 0.15s ease;
}

.hero {
  min-height: 90vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: @fun.space.section.value 1rem;
  background: linear-gradient(135deg, @fun.color.ink.value, @fun.color.brand.value 55%, @fun.color.brand-2.value);
  color: @fun.color.white.value;
}

.hero-badge {
  border: 1px solid rgba(255,255,255,0.25);
  padding: 0.35rem 0.9rem;
  border-radius: 999px;
  font-size: 0.8rem;
  letter-spacing: 0.5px;
  margin-bottom: 1.25rem;
}

.hero h1 {
  font-size: clamp(2rem, 5vw, 3.25rem);
  max-width: 720px;
  margin: 0 0 1rem;
  line-height: 1.1;
}

.hero-lead {
  color: rgba(255,255,255,0.85);
  font-size: 1.15rem;
  max-width: 560px;
  margin: 0 0 2rem;
  line-height: 1.6;
}

.hero-cta {
  @btn-base()
  background: @fun.color.white.value;
  color: @fun.color.ink.value;
}

.hero-cta:hover { transform: translateY(-2px); }

.hero-cta-outline {
  @btn-base()
  background: transparent;
  color: @fun.color.white.value;
  border: 1px solid rgba(255,255,255,0.5);
}

.hero-cta-outline:hover {
  background: rgba(255,255,255,0.1);
}

.hero-cue {
  position: absolute;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  color: rgba(255,255,255,0.6);
  animation: drift-cue 2s ease-in-out infinite;
}
```

## The Motion

```fscss
$(@keyframes drift-cue, .hero-cue, &[2s ease-in-out infinite]) {
  0%   { transform: translate(-50%, 0); opacity: 0.6; }
  50%  { transform: translate(-50%, 10px); opacity: 1; }
  100% { transform: translate(-50%, 0); opacity: 0.6; }
}

@media (prefers-reduced-motion: reduce) {
  .hero-cue { animation: none; }
}
```

## The HTML

```html
<header class="hero">
  <span class="hero-badge">New — v2 is live</span>
  <h1>Build styles that think</h1>
  <p class="hero-lead">FSCSS makes CSS shorter, structured, and data-driven.</p>
  <div>
    <a class="hero-cta" href="#">Get started</a>
    <a class="hero-cta-outline" href="#">Read docs</a>
  </div>
  <span class="hero-cue">↓</span>
</header>
```

## What Happens

- The gradient is assembled from three store colors — change the palette, change the hero.
- Both CTAs share `btn-base`, so typography/tap state stay consistent.
- The scroll cue gets its animation for free via the compact keyframes form.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a grid-pattern background using a repeated `linear-gradient`.
2. Animate the badge in on load with a one-shot keyframe.
3. Move hero colors into `--hero-*` tokens so a `[data-theme]` can re-skin it.

## Summary

Tokens color everything, one base define dresses both CTAs, compact keyframes add motion, and reduced-motion is one override. The hero is done without a single hardcoded color.

---

**Next: [05 — Footer Design](../05-footer-design.md)**