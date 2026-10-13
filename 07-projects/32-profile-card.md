# 32 — Profile Card

A profile card is a résumé in one panel: cover, avatar, stats, and skills. Build it from a token root, a scoped block define for the shell, two array walks (stats and skills), and an `@event` that paints the online/away/offline dot.

## The Brief

- A token root owns surface, ink, muted, and brand colours.
- Cover with an overlapping avatar whose offset is `num()`-derived from its own size.
- A stats row of three columns generated from an array.
- Skill tags tinted per-tag by a hue array stepped through `num()`.
- `@event` branches the presence dot; the CTA pair shares a multiplexed `%2`.

## The FSCSS

```fscss
/* tokens */
@define prof-tokens(root: :root) {
  `
  @use(root){
    --prof-surface: #ffffff;
    --prof-ink: #0f172a;
    --prof-muted: #64748b;
    --prof-brand: #2563eb;
    --prof-bg: #f1f5f9;
    --prof-radius: 18px;
  }
  `
}

@prof-tokens()

/* presence: one event, one dot, three call sites */
@event presence(mode) {
  if mode: online {
    return: #22c55e;
  }
  el-if mode: away {
    return: #f59e0b;
  }
  el {
    return: #94a3b8;
  }
}

/* the shell: one scoped block define */
@define prof-shell(st: .prof-card) {
  `
  @use(st){
    width: 100%;
    max-width: 380px;
    background: var(--prof-surface, #ffffff);
    border-radius: var(--prof-radius, 18px);
    overflow: hidden;
    box-shadow: 0 18px 40px rgba(15, 23, 42, 0.1);
    font-family: system-ui, sans-serif;
  }

  @use(st) .prof-cover {
    height: 150px;
    background: linear-gradient(135deg, var(--prof-brand, #2563eb), #7c3aed);
  }

  @use(st) .prof-body {
    padding: 0 2rem 2rem;
    text-align: center;
  }

  @use(st) .prof-avatar-wrap {
    display: inline-block;
    position: relative;
  }

  @use(st) .prof-avatar {
    size: 120px;
    border-radius: 50%;
    border: 4px solid var(--prof-surface, #ffffff);
    margin-top: num(0 - <120 / 2>)px;
    margin-bottom: 0.75rem;
    -*-user-select: none;
  }

  @use(st) .prof-stats {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    margin: 1.25rem 0;
    border-top: 1px solid rgba(15, 23, 42, 0.06);
    border-bottom: 1px solid rgba(15, 23, 42, 0.06);
  }

  @media (max-width: 420px) {
    @use(st) .prof-tags { flex-wrap: wrap; }
  }
  `
}

@prof-shell()

/* stats: one walk, three columns */
@arr(stats[posts, followers, following])
@arr(si[count(3, 1)])

.prof-stat-@arr.stats[@arr.si[]] {
  padding: 1rem 0;
}

.prof-stat-posts .prof-stat-num {
  color: var(--prof-brand, #2563eb);
}

.prof-stat-num {
  font-size: 1.3rem;
  font-weight: 700;
  color: var(--prof-ink, #0f172a);
}

.prof-stat-label {
  font-size: 0.72rem;
  color: var(--prof-muted, #64748b);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

/* skill tags: a hue per tag via num() at compile time */
@arr(tags[css, sass, dom])
@arr(hues[24, 48, 72])
@arr(ti[count(3, 1)])

.prof-tag-@arr.tags[@arr.ti[]] {
  $idx: @arr.ti[];
  padding: 0.3rem 0.8rem;
  border-radius: 999px;
  font-size: 0.8rem;
  font-weight: 600;
  background: hsl(num(<210 + @arr.hues[$idx!]>) 70% 93%);
  color: hsl(num(<260 + @arr.hues[$idx!]>) 60% 36%);
}

.prof-tags {
  display: flex;
  gap: 0.5rem;
  justify-content: center;
  margin-bottom: 1.5rem;
}

.prof-dot {
  position: absolute;
  size: 16px;
  border-radius: 50%;
  border: 3px solid var(--prof-surface, #ffffff);
  background: @event.presence('offline');
  right: 4px;
  bottom: 4px;
}

.prof-avatar-wrap[data-status="online"] .prof-dot { background: @event.presence('online'); }
.prof-avatar-wrap[data-status="away"] .prof-dot { background: @event.presence('away'); }

.prof-name {
  margin: 0.25rem 0 0.1rem;
  color: var(--prof-ink, #0f172a);
}

.prof-handle {
  color: var(--prof-brand, #2563eb);
  font-size: 0.85rem;
}

.prof-bio {
  color: var(--prof-muted, #64748b);
  font-size: 0.9rem;
  line-height: 1.55;
  margin: 0.5rem 0 1rem;
}

.prof-actions {
  display: flex;
  gap: 0.75rem;
}

.prof-cta {
  %2(width, height[: 42px;])
  flex: 1;
  border-radius: 10px;
  border: none;
  font: inherit;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.15s ease;
}

.prof-cta--primary {
  background: var(--prof-brand, #2563eb);
  color: #ffffff;
}

.prof-cta--ghost {
  background: transparent;
  border: 1px solid var(--prof-brand, #2563eb);
  color: var(--prof-brand, #2563eb);
}

body {
  margin: 0;
  min-height: 100vh;
  display: grid;
  place-items: center;
  padding: 1rem;
  background: var(--prof-bg, #f1f5f9);
  font-family: system-ui, sans-serif;
}
```

`num(0 - <120 / 2>)px` is −60px: the avatar is pushed up exactly half its `size: 120px` diameter, so it straddles the cover boundary, and resizing the avatar resizes the overlap.

## The HTML

```html
<article class="prof-card">
  <div class="prof-cover"></div>
  <div class="prof-body">
    <span class="prof-avatar-wrap" data-status="online">
      <img class="prof-avatar" src="assets/ava.png" alt="Portrait" />
      <span class="prof-dot"></span>
    </span>

    <h1 class="prof-name">Rin Mori</h1>
    <div class="prof-handle">@rinmori</div>
    <p class="prof-bio">FSCSS enthusiast. Styles anything that sits still.</p>

    <div class="prof-stats">
      <div class="prof-stat-posts">
        <div class="prof-stat-num">128</div>
        <div class="prof-stat-label">Posts</div>
      </div>
      <div class="prof-stat-followers">
        <div class="prof-stat-num">4.2k</div>
        <div class="prof-stat-label">Followers</div>
      </div>
      <div class="prof-stat-following">
        <div class="prof-stat-num">312</div>
        <div class="prof-stat-label">Following</div>
      </div>
    </div>

    <div class="prof-tags">
      <span class="prof-tag-css">CSS</span>
      <span class="prof-tag-sass">Sass</span>
      <span class="prof-tag-dom">DOM</span>
    </div>

    <div class="prof-actions">
      <button class="prof-cta prof-cta--primary">Follow</button>
      <button class="prof-cta prof-cta--ghost">Message</button>
    </div>
  </div>
</article>
```

## What Happens

- The token root define writes `--prof-*` once at `:root`, and the shell block define scopes cover, avatar, and stats under `@use(st)` — the card stays one unit.
- The avatar overlap is `num(0 - <120 / 2>)px`, derived by math from the `size: 120px` avatar, so rescaling the art is a single edit.
- Stats and tags are array walks; the tag hues step through `num()` per index, so the third tag's colour is data, not a rule.
- `@event.presence` is one function called for offline/online/away — adding `dnd` is one `el-if` — and `%2(width, height[: 42px;])` gives both CTAs their height in one multiplexed line.
- `-*-user-select: none;` expands the initials area to the full vendor family.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Give `presence` a `dnd` tier (a `#ef4444` dot) with a matching `el-if` and call site.
2. Colour each `.prof-stat-num` from the `hues` array instead of only the posts one.
3. Add a hover ring around the avatar via `-*-transform: scale(1.04)` on `.prof-avatar-wrap`.

## Summary

A cover, a name, three numbers, three tags, and a status dot — all from a token root, two walks, one event, and one scoped define. The profile card is data with a border-radius.

---

**Next: [33 — Chat Interface](../33-chat-interface.md)**