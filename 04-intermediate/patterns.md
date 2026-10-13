# Semantic Patterns (`pattern()`)

`@define` matches by exact name. `pattern()` matches by *meaning*. You describe the style you want in plain words, and FSCSS finds the description with the closest similarity to the phrase it's given.

It's a bit unusual — and genuinely handy for design-language libraries that prefer intent over API.

## The Syntax

```fscss
pattern(threshold: "description", `
  css
`)
```

- `threshold` — a similarity score from `0` to `1` (default `1`). Higher = stricter match.
- `"description"` — the meaning this pattern answers to.
- `` `css` `` — the body emitted when matched.

## A Property Pattern

Define a pattern, then use a natural-language phrase in a rule:

```fscss
pattern(0.5: "Hello World card", `
  border-radius: 12px;
  padding: 24px;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: #fff;
`)

.card {
  hello world card
}
```

`hello world card` scores high enough against the `"Hello World card"` description, so the card gets those declarations.

## Threshold Semantics

| Threshold | Behaviour |
|-----------|-----------|
| `1` (default) | Near-exact phrase match required |
| `0.5` | Tolerant — loose paraphrase is fine |
| `0.1` | Extremely permissive |

Start strict, loosen only when real call sites miss. A threshold that's too low will light up unrelated rules.

## Block Patterns

A block pattern emits a full structure and can stand alone, outside any selector:

```fscss
pattern(0.7: "animated keyframe for spin", `
  @keyframes spin {
    0%   { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
`)

an Animated keyframe for spin
```

The phrase `an Animated keyframe for spin` matches the description and the `@keyframes` block is emitted at the top level.

## Availability

`pattern()` is available from v1.1.25 via CDN, API, and CLI. `@match` support arrived in v1.2.4 — once available, preference goes to explicit matches over fuzzy ones.

## Good Use

- A design-language layer where snippets are referenced by human description ("bouncy enter", "glass panel", "sonar pulse").
- Documentation-as-code: the pattern's description *is* its usage note.
- Prototype phases, where punting on strict naming keeps momentum.

## Caution

- Fuzzy matching is typo-sensitive; a renamed pattern leaves no error until output looks wrong.
- Prefer `@define` for anything with parameters — `pattern()` has none.
- Read your own compiled output after adding patterns; surprises show up there.

## Summary

- `pattern(threshold: "description", \`css\`)` matches on meaning, not exact names.
- Property patterns inject declarations; block patterns emit whole structures.
- Threshold `1` is strict; loosen for paraphrase.
- Great for design-language layers, secondary to `@define` everywhere else.

---

**Next: [Advanced](../05-advanced/README.md)**