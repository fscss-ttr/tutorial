# Vendor Prefixing (`-*-`)

`-webkit-`, `-moz-`, `-ms-`, `-o-` — nobody types them by hand. FSCSS expands a single `-*-` prefix into every vendor form plus the standard property.

## The Syntax

```fscss
.box {
  -*-transform: rotate(45deg);
}
```

Compiles to:

```css
.box {
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  transform: rotate(45deg);
}
```

One declaration, five lines of output. Write the code once; let the preprocessor remember the vendors.

## Where It Shines

Properties that still live partial lives across engines:

```fscss
.card {
  -*-user-select: none;
}

.el {
  -*-backdrop-filter: blur(8px);
}

.grid-item {
  experience: clockwise;
  -*-scroll-snap-align: start;
}
```

`-*-scroll-snap-align` → four vendor properties plus the standard one. If `-o-` isn't supported for that property, the extra line is harmless.

## Mixing with the Standard Property

You never write the unprefixed version yourself — `-*-` includes it. Combining `-*-` and a manual unprefixed declaration is repeating yourself:

```fscss
/* Redundant — -*- already emits `transform` */
.box {
  -*-transform: rotate(45deg);
  transform: rotate(45deg);
}
```

Write just the `-*-` form.

## Combined with Other Syntax

Vendor prefixes play well with the rest of FSCSS — variables, defines, even whole blocks:

```fscss
@define tilt(deg) {
  -*-
  transform: rotate(@use(deg));
}

.logo { @tilt(12deg) }
```

And inside a compact keyframes block:

```fscss
$(@keyframes flip, .star, &[2s ease-in-out infinite]) {
  from { -*-transform: rotateY(0deg); }
  to   { -*-transform: rotateY(180deg); }
}
```

## When Prefixes Disappear

As engines standardize, the `-o-` and `-ms-` lines sometimes drop off. That's fine — the important guarantee is that your source says one thing, and the compiler owns browser trivia.

## Summary

- `-*-property: value;` expands to `-webkit-`, `-moz-`, `-ms-`, `-o-`, and unprefixed.
- One source line; let FSCSS own the vendor matrix.
- `-*-` includes the standard declaration — don't double-write it.

---

**Next: [Semantic Patterns](./patterns.md)**