# Random Values (`@random`)

`@random()` picks one value from a list — at compile time in the CLI, at each render in the browser runtime. Use it for variety: rotation, offsets, tinted variants, the playful bits design systems usually forbid.

## Inline List

```fscss
.btn {
  background: @random([red, blue, green, brown]);
}
```

Each compile (or render) selects a different color — so you can build the same markup shaped differently on each load.

## With Units

The random pick slots into any value:

```fscss
.deck {
  transform: translate(@random([10, 30, 60])px);
}

.tilt {
  rotate: @random([-3, -1, 0, 1, 3])deg;
}
```

## From a Declared Array

Pass an array instead of an inline list:

```fscss
@arr(palette[#4361ee, #f72585, #7209b7]);

.btn {
  border: 2px groove @random(@arr.palette);
}
```

The whole array is the candidate pool; `@random` draws from it.

## Independence

Each `@random()` call draws **independently**. Two calls in one rule are two different picks:

```fscss
.gem {
  background: @random([#ff6b6b, #feca57, #48dbfb]);
  color: @random([#ffffff, #111111]);
}
```

If you want several values to be *related* — the same index across arrays — that is an array-loop job, not `@random`. For unrelated variety, call it freely.

## Controlled Chaos

Random is most useful inside constrained ranges:

```fscss
@arr(spots[0, 12, 24, 36, 48])

.foil {
  background-position: @random(@arr.spots)px @random(@arr.spots)px;
}
```

Only listed offsets, only listed heights. Controlled chaos compiles to something that still looks designed.

## `@random` vs `.randint`

`.randint` is the array method equivalent (`@arr.n!.randint`) — same idea, different entry point. Use the method when you're already in method-access land, and `@random` when you have a raw list or array.

## A Word of Caution

Randomness in UI is a spice, not a diet. Two constraints keep it tasteful:

1. **Keep the pool small and deliberate** — three to eight options.
2. **Keep randomness out of anything that matters** — layout width, readable contrast, interface-critical geometry. Random works on decoration.

## Summary

- `@random([a, b, c])` picks one value from an inline list.
- `@random(@arr.name)` picks from a declared array.
- Runtime mode re-picks each render; the CLI picks once at compile time.
- Each call is independent — keep pools small and use random for decoration.

---

**Next: [Repeat with rpt()](./rpt-and-content.md)**