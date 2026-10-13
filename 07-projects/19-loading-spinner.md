# 19 — Loading Spinner

A spinner is either a circle pretending to be busy or a ring of dots blinking in sequence. Build both from one block define, then let a `count()` loop stagger each dot's delay.

## The Brief

- A ring spinner with a spinning arc.
- A dot spinner whose dots fade in sequence.
- `sm`, `md`, and `lg` sizes driven off one base number.
- An inline variant that pairs a spinner with a label.

## The FSCSS

```fscss
@fun(color){
  track: #e2e8f0;
  accent: #2563eb;
  ink: #0f172a;
  muted: #64748b;
}

@define spin-keyframes() {
  `
  @keyframes fscss-spin {
    to { -*-transform: rotate(360deg); }
  }

  @keyframes fscss-dot {
    0%, 80%, 100% { opacity: 0.25; transform: scale(0.8); }
    40% { opacity: 1; transform: scale(1); }
  }

  @media (prefers-reduced-motion: reduce) {
    .spinner,
    .spinner-dots span { animation: none; }
  }
  `
}

@spin-keyframes()

.spinner {
  display: inline-block;
  size: var(--spinner, 36px);
  border: 4px solid @fun.color.track.value;
  border-top-color: @fun.color.accent.value;
  border-radius: 50%;
  animation: fscss-spin 0.7s linear infinite;
}

@arr(sizes[sm, md, lg])
@arr(px[18, 36, 54])
@arr(s[count(@arr.sizes!.length, 1)])

.spinner-@arr.sizes[@arr.s[]] {
  $i: @arr.s[];
  --spinner: num(@arr.px[$i!])px;
  border-width: num(@arr.px[$i!] / 9)px;
}
```

The dot spinner reuses a count loop to stagger its children:

```fscss
.spinner-dots {
  display: inline-flex;
  align-items: center;
  gap: 6px;
}

.spinner-dots span {
  display: block;
  size: 10px;
  border-radius: 50%;
  background: @fun.color.accent.value;
  animation: fscss-dot 1s ease-in-out infinite;
}

@arr(dots[count(3, 1)])

.spinner-dots span:nth-child(@arr.dots[]) {
  $n: @arr.dots[];
  animation-delay: num(<$n! - 1> * 0.16)s;
}

.loader-inline {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  color: @fun.color.muted.value;
  font-size: 0.9rem;
}
```

## The HTML

```html
<span class="spinner spinner-md" role="status" aria-label="Loading"></span>

<div class="spinner-dots" role="status" aria-label="Loading">
  <span></span>
  <span></span>
  <span></span>
</div>

<span class="loader-inline">
  <span class="spinner spinner-sm" aria-hidden="true"></span>
  Saving changes…
</span>
```

## What Happens

- `spin-keyframes` emits both `@keyframes` blocks and the reduced-motion guard in one block define — motion stays in one place.
- The size walk sets a single `--spinner` custom property, so the ring, and anything else reading that token, resize together.
- `num(px / 9)` scales border thickness with diameter, so a 54px spinner gets a 6px ring without a second array.
- The dot loop writes one `:nth-child` rule per dot, each delay computed from its index with `num(<$n! - 1> * 0.16)s`.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a `spinner-bars` variant that reuses the same `fscss-dot` timing on three rectangles.
2. Hold the loader invisible for 400ms so fast responses never flash it.
3. Read the accent from `currentColor` so a spinner dropped inside a button inherits its color.

## Summary

One block define owns the motion, one array owns the sizes, and a `count()` walk staggers the dots. Scale math keeps the ring proportional at every diameter.

---

**Next: [20 — Toggle Switch](../20-toggle-switch.md)**
