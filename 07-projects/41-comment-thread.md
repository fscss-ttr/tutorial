# 41 — Comment Thread

A comment thread is a tree pretending to be a list. The trick is indentation that can't drift, avatars that stay a fixed size, and a reply line that connects a child to its parent. Generate the nesting depth from an array and let tokens do the rest.

## The Brief

- Comments with avatar, author, time, and body.
- Nested replies indented by depth.
- A reply connector line on nested items.
- A "load more" footer and a compact variant.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #64748b;
  line: #e2e8f0;
  surface: #ffffff;
  accent: #2563eb;
}

@fun(space){
  avatar: 40;
  indent: 40;
  pad: 1rem;
}

@define comment() {
  display: flex;
  gap: 0.75rem;
  padding: @fun.space.pad.value 0;
  border-bottom: 1px solid @fun.color.line.value;
}

@define avatar() {
  %2(width, height[: num(@fun.space.avatar.value)px;])
  border-radius: 50%;
  background: @fun.color.line.value;
  flex-shrink: 0;
}

.comment-avatar {
  @avatar()
}

.comment-author {
  font-weight: 700;
  color: @fun.color.ink.value;
  font-size: 0.9rem;
}

.comment-time {
  color: @fun.color.muted.value;
  font-size: 0.78rem;
}

.comment-body {
  color: @fun.color.ink.value;
  line-height: 1.55;
  margin-top: 0.15rem;
}
```

Depth is a parallel array. Every indentation is `depth × indent`, computed once per level:

```fscss
/* columns:  depth  |  author          */
@arr depths[0, 1, 1, 2, 2, 3]
@arr authors[Ada, Grace, Alan, Linus, Barbara, Ken]
@arr i[count(@arr.depths!.length, 1)]

@define thread() {
  `
  .thread {
    max-width: 720px;
    margin: 0 auto;
    padding: 1.5rem;
    background: @fun.color.surface.value;
  }

  .thread .comment {
    @comment();
  }

  .comment.depth-@arr.depths[@arr.i[]] {
    margin-left: num(@arr.depths[@arr.i[]] * @fun.space.indent.value)px;
  }

  .comment.depth-1,
  .comment.depth-2,
  .comment.depth-3 {
    position: relative;
  }

  .comment.depth-1::before,
  .comment.depth-2::before,
  .comment.depth-3::before {
    content: "";
    position: absolute;
    left: calc(num(@fun.space.indent.value)px / -2);
    top: 0;
    bottom: 0;
    width: 2px;
    background: @fun.color.line.value;
  }
  `
}

@thread()
```

## The HTML

```html
<div class="thread">
  <article class="comment depth-0">
    <div class="comment-avatar"></div>
    <div>
      <div><span class="comment-author">Ada</span> <span class="comment-time">2h ago</span></div>
      <p class="comment-body">Strong first draft. The token layer reads clean.</p>
    </div>
  </article>
  <article class="comment depth-1">
    <div class="comment-avatar"></div>
    <div>
      <div><span class="comment-author">Grace</span> <span class="comment-time">1h ago</span></div>
      <p class="comment-body">Agreed — only the spacing scale needs a pass.</p>
    </div>
  </article>
  <article class="comment depth-2">
    <div class="comment-avatar"></div>
    <div>
      <div><span class="comment-author">Alan</span> <span class="comment-time">45m ago</span></div>
      <p class="comment-body">I'll take the scale.</p>
    </div>
  </article>
</div>
```

## What Happens

- `@comment()` and `@avatar()` keep the row shape and the fixed avatar size in one place, so nested comments can't accidentally shrink the avatar.
- `depth-*` classes are generated from `@arr.depths[]`, and each level's `margin-left` is `depth × indent` — the connector can never disagree with the indent.
- The reply line is a pseudo-element on any item with depth above zero, so it appears automatically wherever nesting exists.
- The avatar size is unitless in the token and rendered with `num(...)px`, which lets the same number drive `%2(width, height[: ...])`.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a collapsed state using `@event depth` to hide deep replies behind a click.
2. Generate the avatar background colors from a parallel array.
3. Add a reply form styled with the same tokens.

## Summary

Depth as data, indentation as a formula, and one define per repeated shape. The thread stays consistent no matter how deep the replies go.

---

**Next: [42 — Stats Cards](../42-stats-cards.md)**
