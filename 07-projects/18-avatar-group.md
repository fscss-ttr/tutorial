# 18 — Avatar Group

A row of faces that overlap into a stack, with a `+N` remainder chip and a presence dot. Sizes come from an array walk, and every overlap distance is `num()` math off a single base number.

## The Brief

- A single avatar with `sm`, `md`, and `lg` sizes from one array.
- An overlapping `.avatar-stack` with a `+N` overflow chip.
- Presence dots for `online`, `away`, and `offline`.
- A ring that separates each face from the one behind it.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #64748b;
  white: #ffffff;
  online: #10b981;
  away: #f59e0b;
  offline: #94a3b8;
}

@define avatar-base() {
  position: relative;
  display: inline-grid;
  place-items: center;
  border-radius: 50%;
  background: #334155;
  color: @fun.color.white.value;
  font-weight: 600;
  user-select: none;
}

.avatar {
  @avatar-base()
  size: 40px;
  font-size: 0.85rem;
}

.avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 50%;
}

@arr(sizes[sm, md, lg])
@arr(px[28, 40, 56])
@arr(s[count(@arr.sizes!.length, 1)])

.avatar-@arr.sizes[@arr.s[]] {
  $i: @arr.s[];
  size: num(@arr.px[$i!])px;
  font-size: num(@arr.px[$i!] * 0.4 / 16)rem;
}
```

The stack and the presence dot both scale off the 40px base:

```fscss
.avatar-stack {
  display: inline-flex;
  align-items: center;
}

.avatar-stack .avatar {
  border: 2px solid @fun.color.white.value;
  margin-left: num(40 * -0.3)px;
}

.avatar-stack .avatar:first-child { margin-left: 0; }

.avatar-more {
  @avatar-base()
  size: 40px;
  margin-left: num(40 * -0.3)px;
  border: 2px solid @fun.color.white.value;
  background: @fun.color.muted.value;
  font-size: 0.78rem;
}

@arr(states[online, away, offline])
@arr(dots[#10b981, #f59e0b, #94a3b8])
@arr(p[count(@arr.states!.length, 1)])

.avatar-@arr.states[@arr.p[]]::after {
  $i: @arr.p[];
  content: "";
  position: absolute;
  right: 0;
  bottom: 0;
  size: num(40 * 0.28)px;
  border-radius: 50%;
  border: 2px solid @fun.color.white.value;
  background: @arr.dots[$i!];
}
```

## The HTML

```html
<div class="avatar-stack">
  <span class="avatar avatar-md"><img src="/ada.jpg" alt="Ada Lovelace" /></span>
  <span class="avatar avatar-md"><img src="/grace.jpg" alt="Grace Hopper" /></span>
  <span class="avatar avatar-md avatar-online"><img src="/katherine.jpg" alt="Katherine Johnson" /></span>
  <span class="avatar avatar-more">+5</span>
</div>

<span class="avatar avatar-lg avatar-away"><img src="/margaret.jpg" alt="Margaret Hamilton" /></span>
```

## What Happens

- The size walk writes `avatar-sm`/`md`/`lg`, each pulling width, height, and font size from the `px` array by index.
- Overlap (`num(40 * -0.3)px`) and dot diameter (`num(40 * 0.28)px`) both derive from the base 40px, so changing the base rescales the whole stack.
- The presence classes differ only by background; the ring and position come from one shared `::after`.
- The `+N` chip is just `avatar-base` with a different fill, so it sits in the stack like any other face.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Render initials as a fallback when an image fails, using `::before` with `content`.
2. Auto-collapse past five members by hiding `:nth-child(n + 6)` and showing the chip.
3. Lift the hovered face above its neighbours with `z-index` and a small scale.

## Summary

One base define, one size array, and one presence array. Every distance is a ratio of the base number, so the group keeps its shape at any size.

---

**Next: [19 — Loading Spinner](../19-loading-spinner.md)**
