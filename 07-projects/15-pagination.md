# 15 — Pagination

Pagination is a short row of page numbers between previous and next. Build the numbered buttons from a counter, size them with `num()`, and mark the current page with one class.

## The Brief

- Previous, a run of page numbers, and next.
- Page-number classes generated from a counter.
- The current page gets an `is-active` fill.
- Buttons squared and rounded consistently with `num()`.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #64748b;
  line: #e2e8f0;
  accent: #2563eb;
  white: #ffffff;
}

$size: num(2 * 18)px;

.pagination {
  display: flex;
  align-items: center;
  gap: 0.35rem;
}

.page-step {
  height: $size!;
  display: inline-flex;
  align-items: center;
  padding: 0 0.75rem;
  border: 1px solid @fun.color.line.value;
  border-radius: num(8 / 2)px;
  color: @fun.color.muted.value;
  text-decoration: none;
  font-size: 0.85rem;
}

.page-step:hover {
  border-color: @fun.color.accent.value;
  color: @fun.color.accent.value;
}
```

## The Data

The numbers are just a counter — six of them — and the loop stamps one class per page:

```fscss
@arr(pages[count(6, 1)])
@arr(pi[count(@arr.pages!.length, 1)])

.page-@arr.pages[@arr.pi[]] {
  width: $size!;
  height: $size!;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: 1px solid @fun.color.line.value;
  border-radius: num(8 / 2)px;
  color: @fun.color.muted.value;
  font-size: 0.9rem;
  text-decoration: none;
  transition: border-color 0.15s ease, color 0.15s ease;
}

.page-@arr.pages[@arr.pi[]]:hover {
  border-color: @fun.color.accent.value;
  color: @fun.color.accent.value;
}

.page-@arr.pages[@arr.pi[]].is-active {
  background: @fun.color.accent.value;
  border-color: @fun.color.accent.value;
  color: @fun.color.white.value;
}
```

## The HTML

```html
<nav class="pagination" aria-label="Pagination">
  <a class="page-step" href="#">Prev</a>
  <a class="page-1" href="#">1</a>
  <a class="page-2 is-active" href="#" aria-current="page">2</a>
  <a class="page-3" href="#">3</a>
  <a class="page-4" href="#">4</a>
  <a class="page-5" href="#">5</a>
  <a class="page-6" href="#">6</a>
  <a class="page-step" href="#">Next</a>
</nav>
```

## What Happens

- `count(6, 1)` produces the run of page numbers, and the walk stamps one class per number.
- `num()` computes the button size and radius once into `$size`, so every button matches.
- `is-active` is the only state class; the current page and hover share the accent.
- The class names carry the page number, so the markup reads like the URL it points to.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Truncate a longer run to `1 … 4 5 6 … 20` with a second array.
2. Style `aria-disabled` on the first and last page steps.
3. Read `?page=` and add `is-active` by comparing it to the class name.

## Summary

A counter builds the numbers, `num()` sizes them, and one active class marks the present. Pagination is data, not markup.

---

**Next: [16 — Badges and Tags](../16-badge-tag.md)**
