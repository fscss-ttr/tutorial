# 34 — Music Player

A player's one job is to look like music is happening, which is a fine excuse for generated equalizer bars and transport controls. Build it from a token root, array walks for the bars and buttons, an `@event` that drives paused vs playing, and build-time `num()` for the seek math.

## The Brief

- A token root owns the warm palette, brand violet, and radius.
- A five-bar equalizer generated from arrays, with `num()`-scaled heights and staggered delays; bars freeze when the player pauses.
- Transport row (skip-back, play, skip-fwd) generated from an array, play as the larger accent.
- Seek and volume fills as build-time `num()` subtraction; `@event` shades the play button by state.
- Vendor-prefixed hover lift on the album art and a `size:`-shorthand play button.

## The FSCSS

```fscss
/* tokens */
@define pl-tokens(root: :root) {
  `
  @use(root){
    --pl-bg: #f6f4f0;
    --pl-surface: #ffffff;
    --pl-ink: #1c1917;
    --pl-muted: #78716c;
    --pl-brand: #8b5cf6;
    --pl-radius: 22px;
  }
  `
}

@pl-tokens()

/* two state events, each a single decision */
@event eq-run(mode) {
  if mode: paused {
    return: paused;
  }
  el {
    return: running;
  }
}

@event ctrl-tone(state) {
  if state: playing {
    return: #6d28d9;
  }
  el {
    return: var(--pl-brand, #8b5cf6);
  }
}

/* the equalizer: five bars, height and delay as data */
@arr(bars[1, 2, 3, 4, 5])
@arr(hs[8, 16, 22, 12, 18])
@arr(dl[0, 0.1, 0.2, 0.3, 0.4])
@arr(ei[count(5, 1)])

.eq-bar-@arr.bars[@arr.ei[]] {
  $idx: @arr.ei[];
  width: 6px;
  height: num(2 * @arr.hs[$idx!])px;
  border-radius: 3px;
  background: #ffffff;
  transform-origin: bottom;
  animation: eq-bounce 0.9s ease-in-out infinite alternate;
  animation-delay: @arr.dl[$idx!]s;
  animation-play-state: @event.eq-run('running');
}

.player.is-paused .eq-bar-@arr.bars[@arr.ei[]] {
  animation-play-state: @event.eq-run('paused');
}

/* the transport: three buttons from one walk */
@arr(btns[prev, play, next])
@arr(bi[count(3, 1)])

.ctrl-@arr.btns[@arr.bi[]] {
  size: 44px;
  border: none;
  border-radius: 50%;
  display: grid;
  place-items: center;
  background: transparent;
  color: var(--pl-ink, #1c1917);
  cursor: pointer;
}

.ctrl-play {
  size: 58px;
  background: @event.ctrl-tone('idle');
  color: #ffffff;
  box-shadow: 0 8px 20px rgba(139, 92, 246, 0.35);
}

.player.is-playing .ctrl-play {
  background: @event.ctrl-tone('playing');
}

/* the shell: one scoped block define, breakpoint included */
@define pl-shell(st: .player) {
  `
  @use(st){
    width: 100%;
    max-width: 340px;
    margin: 2rem auto;
    padding: 2rem 1.5rem 1.75rem;
    border-radius: var(--pl-radius, 22px);
    background: var(--pl-surface, #ffffff);
    box-shadow: 0 24px 60px rgba(28, 25, 23, 0.12);
    font-family: system-ui, sans-serif;
    color: var(--pl-ink, #1c1917);
  }

  @media (max-width: 380px) {
    @use(st){
      max-width: none;
      margin: 0;
      border-radius: 0;
    }
  }
  `
}

@pl-shell()

.album {
  position: relative;
  size: 240px;
  margin: 0 auto;
  border-radius: 18px;
  background: linear-gradient(135deg, #7c3aed, var(--pl-brand, #8b5cf6));
  overflow: hidden;
  transition: transform 0.2s ease;
}

.album:hover {
  -*-transform: translateY(-4px);
}

.eq {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  gap: 5px;
  padding-bottom: 18px;
}

.meta {
  text-align: center;
  margin: 1.5rem 0 0.5rem;
}

.meta-title {
  font-size: 1.15rem;
  font-weight: 700;
  color: var(--pl-ink, #1c1917);
}

.meta-artist {
  font-size: 0.85rem;
  color: var(--pl-muted, #78716c);
}

.transport {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  margin: 1.25rem 0;
}

.seek {
  position: relative;
  height: 6px;
  border-radius: 3px;
  background: #e7e5e4;
}

.seek-fill {
  position: absolute;
  inset: 0 auto 0 0;
  width: num(100 - 38)%;
  border-radius: 3px;
  background: var(--pl-brand, #8b5cf6);
}

.seek-thumb {
  position: absolute;
  size: 14px;
  border-radius: 50%;
  background: #ffffff;
  border: 2px solid var(--pl-brand, #8b5cf6);
  top: -4px;
  left: num(<100 - 38> - 2)%;
}

.time-row {
  display: flex;
  justify-content: space-between;
  font-size: 0.75rem;
  color: var(--pl-muted, #78716c);
  margin: 0.5rem 0 1rem;
}

.volume {
  display: flex;
  align-items: center;
  gap: 0.7rem;
}

.volume-lbl {
  font-size: 0.72rem;
  font-weight: 600;
  color: var(--pl-muted, #78716c);
  letter-spacing: 0.5px;
}

.volume-bar {
  flex: 1;
  height: 4px;
  border-radius: 2px;
  background: #e7e5e4;
}

.volume-fill {
  width: num(100 - 30)%;
  height: 100%;
  border-radius: 2px;
  background: var(--pl-brand, #8b5cf6);
}

body {
  margin: 0;
  min-height: 100vh;
  display: grid;
  place-items: center;
  background: var(--pl-bg, #f6f4f0);
}

/* bar bounce: the compact keyframes macro pairs it with the first bar */
$(@keyframes eq-bounce, .eq-bar-1, &[0.9s ease-in-out infinite alternate]) {
  from { transform: scaleY(0.4); }
  to { transform: scaleY(1); }
}
```

`num(2 * @arr.hs[$idx!])px` scales each bar's data height by two at compile time, and the seek/volume fills are `num(100 - 38)%` and `num(100 - 30)%` — the author writes the unwatched remainder, not the painted length.

## The HTML

```html
<div class="player">
  <div class="album">
    <div class="eq">
      <span class="eq-bar-1"></span>
      <span class="eq-bar-2"></span>
      <span class="eq-bar-3"></span>
      <span class="eq-bar-4"></span>
      <span class="eq-bar-5"></span>
    </div>
  </div>

  <div class="meta">
    <div class="meta-title">Night Desk</div>
    <div class="meta-artist">Mono Aura</div>
  </div>

  <div class="transport">
    <button class="ctrl-prev" aria-label="Previous">«</button>
    <button class="ctrl-play" aria-label="Play">▶</button>
    <button class="ctrl-next" aria-label="Next">»</button>
  </div>

  <div class="seek">
    <div class="seek-fill"></div>
    <div class="seek-thumb"></div>
  </div>

  <div class="time-row">
    <span>1:26</span>
    <span>3:42</span>
  </div>

  <div class="volume">
    <span class="volume-lbl">Vol</span>
    <div class="volume-bar"><div class="volume-fill"></div></div>
  </div>
</div>
```

## What Happens

- The equalizer is one array walk: height comes from `num(2 * @arr.hs[$idx!])px`, delay from the `dl` array, and a second paused-state walk re-calls `@event.eq-run('paused')` to freeze every bar at once.
- The transport row is a second walk with `ctrl-play` overriding the base `size: 44px` up to 58px; `@event.ctrl-tone` shades the play button between idle and playing.
- Seek fill, thumb, and volume fill are build-time `num()` subtraction, so the author edits the remainder and the compiler emits the resolved percentage.
- The album's hover lift uses the `-*-transform` macro, which expands to the webkit, moz, ms, and o families plus the standard property.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add shuffle/repeat buttons from a `@arr(modes[shuffle, repeat, queue])` walk, tinted by a third `@event` tier.
2. Fold the paused flicker plus play-tone switch into one `@define play-skin()` so `.is-paused` changes skin in one call.
3. Give the seek thumb a hover ring and scale it with `-*-transform: scale(1.15)`.

## Summary

Five bars, three buttons, two fills, zero duplicated math. The equalizer and controls are raw data walked into rules, the state lives in two events, and *Music Player* compiles to CSS that looks alive — then, politely, pauses.

---

**Next: [35 — Video Player UI](../35-video-player-ui.md)**