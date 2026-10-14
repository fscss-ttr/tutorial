# Animation Techniques

FSCSS doesn't change what browsers can animate — it changes how much typing the animation costs. Compact keyframes, generated staggers, and tokenized timing make motion cheap to author and easy to theme.

## Compact Keyframes

The shorthand form defines the keyframes and applies the animation in one block:

```fscss
$(@keyframes slideIn, .box, .card, &[3s linear infinite]) {
  from { transform: translateX(-100%); }
  to   { transform: translateX(0); }
}
```

```css
.box, .card {
  animation: slideIn 3s linear infinite;
}
@keyframes slideIn {
  from { transform: translateX(-100%); }
  to   { transform: translateX(0); }
}
```

Shape: `$(@keyframes name, selector-one, selector-two, &[duration timing iteration])`.

## Keyframes in a Block Define

Reusable animation suites:

```fscss
@define float-on(duration: 1s) {
  `
  @keyframes float-on {
    0%   { transform: translateY(0); }
    50%  { transform: translateY(-8px); }
    100% { transform: translateY(0); }
  }
  .floating {
    animation: float-on @use(duration) ease-in-out infinite;
  }
  `
}

@float-on(1.4s)
```

## Stagger — The Classic Array Trick

Staggered entrances are the single best use of arrays in animation:

```fscss
@arr(delays[0s, 80ms, 160ms, 240ms, 320ms])
@arr(i[count(5, 1)])

.item:nth-child(@arr.i[]) {
  animation: rise 0.5s ease-out both;
  animation-delay: @arr.delays[@arr.i[]];
}
```

```css
.item:nth-child(1) { animation: rise .5s ease-out both; animation-delay: 0s; }
.item:nth-child(2) { animation: rise .5s ease-out both; animation-delay: 80ms; }
/* …
```

Five cards, one stagger, no hand-writing. Better: name the delay unit via `@arr.delays!.unit(ms)`.

## Tokenized Timing

Put motion in tokens so preferences (and brand pacing) are one edit:

```fscss
:root {
  --speed-fast: 120ms;
  --speed-base: 250ms;
  --ease-out: cubic-bezier(0.22, 1, 0.36, 1);
}

.card {
  transition: transform var(--speed-base) var(--ease-out);
}
```

Reduce-it users get control through the same tokens:

```fscss
@media (prefers-reduced-motion: reduce) {
  :root {
    --speed-base: 1ms;
  }
}
```

(`both`/`backwards` fills keep entrance animations safe even when shortened.)


## Discipline

- Animate `transform` and `opacity`; let the compositor do the rest.
- Favor tokens for speed/easing so motion is themeable.
- Provide a reduced-motion path via token overrides.
- Preview compiled keyframes once — compact forms hide generator bugs.

## Summary

- `$(@keyframes name, selectors, &[dur timing iteration])` defines + applies in one block.
- Block defines package whole animation suites.
- Arrays + `count()` drive staggered delays.
- Tokenize speed and easing; honor `prefers-reduced-motion`.

---

**Next: [Building Component Libraries](./component-library.md)**
