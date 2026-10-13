# 35 — Video Player UI

A video player is the last place you want a wall of hardcoded pixel offsets. Build a 16:9 frame with a generated chapter-ticks line, an `@event`-driven overlay, a `str()` block for the control scrim, and build-time `num()` widths for fill, buffer, and thumb.

## The Brief

- A scoped block define builds the 16:9 frame, with a phone breakpoint that strips the chrome.
- Three chapter ticks generated from an array and positioned by `num()` from their own index.
- `@event` fades the big play overlay between paused and playing.
- A `str()` block injects the gradient scrim and padding into the control bar.
- Seek fill/buffer/thumb and volume are build-time `num()` subtraction; vendor prefixes on the player and the overlay button.

## The FSCSS

```fscss
/* tokens */
@define vp-tokens(root: :root) {
  `
  @use(root){
    --vp-bg: #0a0e17;
    --vp-ink: #eef2f7;
    --vp-muted: #94a3b8;
    --vp-accent: #38bdf8;
    --vp-live: #ef4444;
    --vp-track: rgba(148, 163, 184, 0.35);
  }
  `
}

@vp-tokens()

/* state decision: one event, two call sites */
@event overlay-state(state) {
  if state: playing {
    return: 0;
  }
  el {
    return: 1;
  }
}

/* the control scrim: a named block string, injected bare */
str(scrim, "background: linear-gradient(180deg, transparent, rgba(10, 14, 23, 0.75)); padding: 2.25rem 1.25rem 1rem;")

/* chapter ticks: one walk, position from num() */
@arr(chaps[intro, build, wrap])
@arr(ci[count(3, 1)])

.seek-chap-@arr.chaps[@arr.ci[]] {
  $idx: @arr.ci[];
  position: absolute;
  top: -3px;
  left: num(<25 * $idx!> - 12)%;
}

.seek-chap-@arr.chaps[@arr.ci[]]::before {
  content: "";
  size: 8px;
  border-radius: 50%;
  background: var(--vp-accent, #38bdf8);
}

.seek-chap-@arr.chaps[@arr.ci[]]::after {
  content: "@arr.chaps[@arr.ci[]]";
  position: absolute;
  top: 14px;
  left: -20px;
  font-size: 0.66rem;
  color: var(--vp-ink, #eef2f7);
  opacity: 0.85;
  pointer-events: none;
}

/* the frame: one scoped block define, breakpoint included */
@define vp-shell(st: .vplayer) {
  `
  @use(st){
    position: relative;
    width: 100%;
    max-width: 860px;
    aspect-ratio: 16 / 9;
    margin: 2rem auto;
    overflow: hidden;
    border-radius: 16px;
    background: var(--vp-bg, #0a0e17);
    font-family: system-ui, sans-serif;
    -*-user-select: none;
  }

  @use(st) video {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  @media (max-width: 640px) {
    @use(st){
      border-radius: 0;
      margin: 0;
    }
    @use(st) .volume { display: none; }
  }
  `
}

@vp-shell()

.vp-top {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem 1.25rem;
}

.vp-title {
  font-weight: 700;
  color: var(--vp-ink, #eef2f7);
  text-shadow: 0 1px 8px rgba(0, 0, 0, 0.5);
}

.vp-live {
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
  background: var(--vp-live, #ef4444);
  color: #ffffff;
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 1px;
  animation: live-pulse 1.6s ease-in-out infinite;
}

.big-play {
  position: absolute;
  inset: 0;
  display: grid;
  place-items: center;
  background: rgba(10, 14, 23, 0.35);
  border: none;
  cursor: pointer;
  transition: opacity 0.25s ease;
  opacity: @event.overlay-state('paused');
}

.big-play[data-state="playing"] {
  opacity: @event.overlay-state('playing');
  pointer-events: none;
}

.big-play-btn {
  width: 84px;
  height: 84px;
  border: none;
  border-radius: 50%;
  background: var(--vp-accent, #38bdf8);
  color: var(--vp-bg, #0a0e17);
  font-size: 1.9rem;
  cursor: pointer;
  transition: transform 0.15s ease;
}

.big-play-btn:hover {
  -*-transform: scale(1.06);
}

.vp-controls {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  scrim
}

.seek-track {
  position: relative;
  height: 6px;
  border-radius: 3px;
  background: var(--vp-track, rgba(148, 163, 184, 0.35));
  margin-bottom: 0.9rem;
  cursor: pointer;
}

.seek-buffer {
  position: absolute;
  inset: 0 auto 0 0;
  width: num(100 - 26)%;
  border-radius: 3px;
  background: rgba(148, 163, 184, 0.5);
}

.seek-fill {
  position: absolute;
  inset: 0 auto 0 0;
  width: num(100 - 69)%;
  border-radius: 3px;
  background: var(--vp-accent, #38bdf8);
}

.seek-thumb {
  position: absolute;
  size: 12px;
  border-radius: 50%;
  background: var(--vp-accent, #38bdf8);
  top: -3px;
  left: num(<100 - 69> - 2)%;
}

.controls-row {
  display: flex;
  align-items: center;
  gap: 0.9rem;
  color: var(--vp-ink, #eef2f7);
}

.ctrl-btn {
  background: none;
  border: none;
  padding: 0.25rem;
  color: var(--vp-ink, #eef2f7);
  font-size: 1rem;
  cursor: pointer;
}

.time {
  font-size: 0.78rem;
  color: var(--vp-ink, #eef2f7);
  font-variant-numeric: tabular-nums;
}

.volume {
  margin-left: auto;
  display: flex;
  align-items: center;
  gap: 0.4rem;
}

.vol-bar {
  width: 56px;
  height: 4px;
  border-radius: 2px;
  background: var(--vp-track, rgba(148, 163, 184, 0.35));
}

.vol-fill {
  width: num(140 / 2)%;
  height: 100%;
  border-radius: 2px;
  background: var(--vp-ink, #eef2f7);
}

body {
  margin: 0;
  min-height: 100vh;
  display: grid;
  place-items: center;
  background: #f1f5f9;
}

/* the live pulse: compact keyframes macro attached to the badge */
$(@keyframes live-pulse, .vp-live, &[1.6s ease-in-out infinite]) {
  0%, 100% { opacity: 0.9; }
  50% { opacity: 0.5; }
}
```

The chapter ticks sit at 13%, 38%, and 63% — `num(<25 * $idx!> - 12)%` for `$idx!` of 1, 2, 3 — and their labels are array values read back into `content`.

## The HTML

```html
<section class="vplayer">
  <video muted loop></video>

  <div class="vp-top">
    <span class="vp-title">Lecture 12 — Layout Systems</span>
    <span class="vp-live">LIVE</span>
  </div>

  <button class="big-play" data-state="paused" aria-label="Play">
    <span class="big-play-btn">▶</span>
  </button>

  <div class="vp-controls">
    <div class="seek-track">
      <div class="seek-buffer"></div>
      <div class="seek-fill"></div>
      <span class="seek-chap-intro"></span>
      <span class="seek-chap-build"></span>
      <span class="seek-chap-wrap"></span>
      <span class="seek-thumb"></span>
    </div>

    <div class="controls-row">
      <button class="ctrl-btn" aria-label="Play">▶</button>
      <span class="time"><span class="time-current">1:24</span> / <span class="time-total">12:40</span></span>
      <div class="volume">
        <span class="vol-bar"><span class="vol-fill"></span></span>
      </div>
      <button class="ctrl-btn" aria-label="Fullscreen">FS</button>
    </div>
  </div>
</section>
```

## What Happens

- The three chapter ticks come from one array walk: `left` is `num(<25 * $idx!> - 12)%`, so moving the chapters means editing the array, not three rules.
- `str(scrim, …)` holds the gradient plus padding in one named block injected with a bare `scrim` — the control bar can't drift from its scrim.
- `@event.overlay-state` returns the opacity for both the paused default and the playing call site; the same playing rule kills pointer-events so the bar beneath stays clickable.
- Fill, buffer, thumb, and volume are build-time `num()` subtraction; `-*-transform` and `-*-user-select` expand to their vendor families.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Generate the chapter positions from a `@arr(starts[912, 431, 684])` array of seconds via `num()` instead of authored percentages.
2. Add an `error` tier and a `buffering` tier to `overlay-state` so the overlay reopens by reason, not just state.
3. Build a transcript side panel from a shared `@arr(lines[next, ...])` walk and a block define, synced to the ticks.

## Summary

One event fades the overlay, one array places the ticks, one `str()` owns the scrim, and `num()` handles every width. The player is a frame, some data, and a decision — exactly as unwieldy as it needs to be.

---

**Next: [36 — Calendar Widget](../36-calendar-widget.md)**