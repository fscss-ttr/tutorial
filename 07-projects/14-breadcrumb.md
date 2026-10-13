# 14 — Breadcrumb

A breadcrumb is a trail of links whose separators must not follow the last item. Generate the trail's classes from an array and let CSS draw the slashes.

## The Brief

- A horizontal trail of links separated by `/`.
- The last crumb is the current page: bold, unlinked, no separator.
- Crumb and separator rules generated from one array.
- Uniform spacing computed with `num()`.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #64748b;
  accent: #2563eb;
  line: #cbd5e1;
}

@arr(trail[home, library, components, breadcrumb])
@arr(ti[count(@arr.trail!.length, 1)])

.breadcrumb {
  display: flex;
  align-items: center;
  padding: 0.75rem 1rem;
  background: #f8fafc;
  border-radius: 8px;
}

.crumb-@arr.trail[@arr.ti[]] {
  color: @fun.color.muted.value;
  text-decoration: none;
  font-size: 0.9rem;
}

.crumb-@arr.trail[@arr.ti[]]:hover {
  color: @fun.color.accent.value;
  text-decoration: underline;
}

.crumb-@arr.trail[@arr.ti[]]::after {
  content: "/";
  margin: 0 num(0.5 * 2)rem;
  color: @fun.color.line.value;
}

.crumb-@arr.trail[@arr.ti[]]:last-child {
  color: @fun.color.ink.value;
  font-weight: 600;
  pointer-events: none;
}

.crumb-@arr.trail[@arr.ti[]]:last-child::after {
  content: "";
  margin: 0;
}
```

## The HTML

```html
<nav class="breadcrumb" aria-label="Breadcrumb">
  <a class="crumb-home" href="#">Home</a>
  <a class="crumb-library" href="#">Library</a>
  <a class="crumb-components" href="#">Components</a>
  <span class="crumb-breadcrumb" aria-current="page">Breadcrumb</span>
</nav>
```

## What Happens

- The array drives all four crumb classes; the separator is a `::after` that every crumb carries.
- `:last-child` clears the content and margin, so the slash never trails the current page.
- `num()` computes the gap once, so the rhythm between crumbs stays even.
- The current page is styled by position, not by an extra class.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Swap the `/` for an SVG chevron via a `mask` on the pseudo-element.
2. Collapse a long trail to `Home / … / Current` under 600px.
3. Emit the whole list from a `str()` block built off the same array.

## Summary

Position does the work here: the array sizes the trail, `:last-child` marks the current page, and `num()` keeps the gaps even.

---

**Next: [15 — Pagination](../15-pagination.md)**
