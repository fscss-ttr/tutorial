# 10 — Progress Bar

A progress bar is a track and a fill. Build the track once, then drive the fill widths from an array so a set of status levels is data rather than five copy-pasted rules.

## The Brief

- A rounded track and a fill with a smooth width transition.
- Colour variants for success and warning contexts.
- Determinate fills in a few named levels.
- An indeterminate, sliding state.

## The FSCSS

```fscss
@fun(color){
  track: #e2e8f0;
  primary: #2563eb;
  success: #10b981;
  warning: #d97706;
  ink: #0f172a;
  muted: #64748b;
}

$height: 0.6rem;

@define bar() {
  height: $height!;
  background: @fun.color.track.value;
  border-radius: 999px;
  overflow: hidden;
}

.bar { @bar() }

.bar-fill {
  height: 100%;
  background: @fun.color.primary.value;
  border-radius: 999px;
  transition: width 0.4s ease;
}

.bar-fill--success { background: @fun.color.success.value; }
.bar-fill--warning { background: @fun.color.warning.value; }

.bar-label {
  display: flex;
  justify-content: space-between;
  font-size: 0.8rem;
  color: @fun.color.muted.value;
  margin-bottom: 0.35rem;
}

.bar-value {
  color: @fun.color.ink.value;
  font-weight: 600;
}
```

## The Data

The named levels are two parallel arrays. One `@arr` walk writes every width:

```fscss
@arr(levels[low, mid, high, full])
@arr(w[20, 45, 70, 100])
@arr i[count(@arr.w!.length, 1)]

.bar-fill--@arr.levels[@arr.i[]] {
  $idx: @arr.i[];
  width: num(@arr.w[$idx!] * 1)%;
}
```

Indeterminate motion is raw CSS, so it lives in a block define:

```fscss
@define bar-stripes() {
  `
  .bar-indeterminate .bar-fill {
    width: 40%;
    animation: slide 1.2s ease-in-out infinite;
  }

  @keyframes slide {
    0%   { transform: translateX(-100%); }
    100% { transform: translateX(250%); }
  }
  `
}

@bar-stripes()
```

## The HTML

```html
<div>
  <div class="bar-label">
    <span>Uploading</span>
    <span class="bar-value">70%</span>
  </div>
  <div class="bar">
    <div class="bar-fill bar-fill--high"></div>
  </div>
</div>

<div style="margin-top: 1.5rem">
  <div class="bar">
    <div class="bar-fill bar-fill--success bar-fill--full"></div>
  </div>
</div>

<div class="bar bar-indeterminate" style="margin-top: 1.5rem">
  <div class="bar-fill"></div>
</div>
```

## What Happens

- `bar` is the track define; the fill is a separate, independently colourable rule.
- `@arr` + `count()` generate four width classes, so adding a level is one array entry.
- `num(@arr.w[$idx!] * 1)%` turns the raw number into a percentage at build time.
- The block define writes `@keyframes` and the indeterminate rule, which a declarations-only define could not.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a circular `stroke-dasharray` variant using the same level array.
2. Show a striped-and-animated variant that combines the two states.
3. Drive the `bar-value` text width with `rpt()` for a repeating tick pattern.

## Summary

Tokens for the colours, `$height` for the one dimension that matters, arrays for the levels. The bar stays a bar however many states you add.

---

**Next: [11 — Tabs Component](../11-tabs-component.md)**
