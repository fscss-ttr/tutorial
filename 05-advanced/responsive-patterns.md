# Responsive Patterns

Block defines and tokens make responsive CSS pleasant: breakpoints become named, reusable blocks, and your "design to tokens, not values" habit extends to layout.

## The Breakpoint Store

Name your breakpoints once, read them anywhere:

```fscss
@fun(bp){
  sm: 480px;
  md: 768px;
  lg: 1024px;
  xl: 1280px;
}
```

```fscss
@media (min-width: @fun.bp.md.value) {
  .grid { grid-template-columns: repeat(2, 1fr); }
}
```

Changing a breakpoint is now a one-line edit, not a find-and-replace.

## A Breakpoint Define

Name the intent, not the pixel club, with block defines:

```fscss
@define from(bp) {
  `
  @media (min-width: @use(bp)) {
    @use(bp-body)
  }
  `
}
```

A cleaner, less recursive version — flat and honest:

```fscss
@define tablet() {
  `
  @media (min-width: 768px) {
    .grid { grid-template-columns: repeat(2, 1fr); }
    .nav  { display: flex; }
  }
  `
}

@tablet()
```

## Mobile-First vs Desktop-First

Mobile-first (defaults are the mobile layout, `min-width` queries add more):

```fscss
.grid {
  grid-template-columns: 1fr;          /* mobile */
}

@media (min-width: 768px) {
  .grid { grid-template-columns: repeat(2, 1fr); }
}

@media (min-width: 1024px) {
  .grid { grid-template-columns: repeat(4, 1fr); }
}
```

Desktop-first (desktop default, `max-width` removes features):

```fscss
.nav { gap: 2rem; }

@media (max-width: 767px) {
  .nav { flex-direction: column; gap: 0.5rem; }
}
```

Mobile-first is the default recommendation: simpler cascade, less to undo.

## Generated Breakpoint Helpers

Loop breakpoints into utilities:

```fscss
@arr(bp[sm, md, lg])
@arr(px[480, 768, 1024])
@arr(i[count(3, 1)])

@media (min-width: @arr.px[@arr.i[]]px) {
  .show-@arr.bp[@arr.i[]] { display: block; }
}
```

```css
@media (min-width: 480px)  { .show-sm { display: block; } }
@media (min-width: 768px)  { .show-md { display: block; } }
@media (min-width: 1024px) { .show-lg { display: block; } }
```

## Container Queries, Tokenizably

Container queries respond to the container, not the viewport. FSCSS has no special syntax — write them normally in a block define:

```fscss
@define panel-layout() {
  `
  .panel {
    container-type: inline-size;
  }
  @container (min-width: 480px) {
    .panel .meta { display: grid; grid-template-columns: auto 1fr; }
  }
  `
}

@panel-layout()
```

## Responsive by Tokens

Let the *token values* change under media queries instead of hunting rules:

```fscss
:root {
  --gap: 1rem;
  --cols: 1;
}

@media (min-width: 768px) {
  :root {
    --gap: 1.5rem;
    --cols: 2;
  }
}

@media (min-width: 1024px) {
  :root { --cols: 4; }
}

.board {
  display: grid;
  grid-template-columns: repeat(var(--cols), 1fr);
  gap: var(--gap);
}
```

Components that read the tokens respond automatically to every breakpoint with zero media queries of their own.

## Checklist

1. Breakpoints live in one `@fun` store.
2. Reusable behaviors are block defines.
3. Mobile-first unless there's a real reason not to.
4. Layout knobs are tokens; breakpoints change tokens.
5. Compile and eyeball each breakpoint once.

## Summary

- Centralize breakpoints in a store or named defines.
- Default to mobile-first `min-width` queries.
- Generate breakpoint utilities with arrays.
- Make layout knobs tokens so components respond without their own queries.

---

**Next: [Animation Techniques](./animation-techniques.md)**