# Random in Depth

Beginner `@random` was enough for a wink of variety. Advanced random is *controlled*: small pools, constrained ranges, and chaos that still reads as design.

## Pick from a Pool

```fscss
@arr(spots[0, 25, 50, 75, 100])

.grain {
  background-position: @random(@arr.spots)% @random(@arr.spots)%;
}
```

Only listed positions, ever. The array is the budget; `@random` spends it.

## Random as Color Casing

Pseudo-random RGB directly:

```fscss
@arr(r[40, 80, 120, 200])
@arr(g[40, 80, 255])
@arr(b[..., ])
```

Define real pools:

```fscss
@arr(r[60, 120, 180, 240])
@arr(g[80, 160, 240])
@arr(b[100, 190, 255])

.pixel {
  background: rgb(@random(@arr.r), @random(@arr.g), @random(@arr.b));
}
```

Still bounded, still colorful. Each compile lands somewhere in that cube.

## Rotation and Tilt Variety

```fscss
@arr(tilt[-4, -2, 0, 2, 4])
@arr(shift[0, 6, 12])

.paper {
  rotate: @random(@arr.tilt)deg;
  translate: @random(@arr.shift)px @random(@arr.shift)px;
}
```

Cumulative randomness makes a stack of papers feel hand-shuffled without looking broken.

## Determinism and Runtime

- **CLI compile:** each `@random()` is picked once, baked into the output. Rebuild → the exact same values again.
- **Browser runtime:** re-picked at every render.

So random is a *build-time* strategy with the CLI but a *live* strategy in the browser. Choose by whether you want stable screenshots (CLI) or fresh screens every load (runtime).

## Shuffle vs Random

`.shuffle` guarantees *twelve cards, all twelve present, in different order*. `@random` can repeat. Use shuffle when distribution matters:

```fscss
@arr(cards[1, 2, 3, 4])
@arr(deck[@arr.cards!.shuffle])

.card { content: "@arr.deck!.list"; }
```

## The Discipline of Bounded Randomness

Good FSCSS random:

- Small pools (3–8 values).
- Values chosen deliberately (even, spaced).
- Applied to decoration: chips, clouds, tints, tilt, offset.
- Never applied to contrast, layout, or critical geometry.

If a random value can break the design, it's not random-worthy — it's a candidate for `@event` per variant instead.

## Summary

- Constrain `@random` to deliberate pools via arrays.
- Compose several picks for tilt/shift/tint variety.
- CLI picks once per build; runtime re-picks per render — choose accordingly.
- `.shuffle` when distribution matters; `@random` for single draws.
- Keep randomness on the decoration layer.

---

**Next: [Math in Depth](./num-advanced.md)**
