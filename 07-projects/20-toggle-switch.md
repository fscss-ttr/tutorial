# 20 — Toggle Switch

A switch is a checkbox wearing a costume. Style the real input, generate size variants from an array, and publish the colors as `:root` tokens so a theme can flip them in one place.

## The Brief

- A `.switch` built on a real `<input type="checkbox">`.
- `sm`, `md`, and `lg` sizes from one array walk.
- On/off colors and a sliding knob, all from tokens.
- A visible focus ring and a paired text label.

## The FSCSS

```fscss
@fun(color){
  off: #cbd5e1;
  on: #2563eb;
  knob: #ffffff;
  ink: #0f172a;
  muted: #64748b;
}

@define switch-tokens(root: :root) {
  `
  @use(root) {
    --switch-on: #2563eb;
    --switch-off: #cbd5e1;
    --switch-knob: #ffffff;
    --switch-focus: #93c5fd;
  }
  `
}

@switch-tokens()

.switch {
  position: relative;
  display: inline-block;
  width: 48px;
  height: 28px;
  vertical-align: middle;
}

.switch input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

.switch-track {
  position: absolute;
  inset: 0;
  background: var(--switch-off, #cbd5e1);
  border-radius: 999px;
  transition: background-color 0.2s ease;
  cursor: pointer;
}

.switch-track::before {
  content: "";
  position: absolute;
  top: 3px;
  left: 3px;
  size: 22px;
  background: var(--switch-knob, #ffffff);
  border-radius: 50%;
  transition: transform 0.2s ease;
}

.switch input:checked + .switch-track {
  background: var(--switch-on, #2563eb);
}

.switch input:checked + .switch-track::before {
  transform: translateX(20px);
}

.switch input:focus-visible + .switch-track {
  outline: 2px solid var(--switch-focus, #93c5fd);
  outline-offset: 2px;
}
```

Every size derives from one number in the array:

```fscss
@arr(sizes[sm, md, lg])
@arr(w[36, 48, 64])
@arr(s[count(@arr.sizes!.length, 1)])

.switch-@arr.sizes[@arr.s[]] {
  $i: @arr.s[];
  width: num(@arr.w[$i!])px;
  height: num(@arr.w[$i!] * 0.58)px;
}

.switch-@arr.sizes[@arr.s[]] .switch-track::before {
  $i: @arr.s[];
  size: num(@arr.w[$i!] * 0.46)px;
  top: num(@arr.w[$i!] * 0.06)px;
  left: num(@arr.w[$i!] * 0.06)px;
}

.switch-@arr.sizes[@arr.s[]] input:checked + .switch-track::before {
  $i: @arr.s[];
  transform: translateX(num(@arr.w[$i!] * 0.42)px);
}

.switch-label {
  color: @fun.color.ink.value;
  font-size: 0.9rem;
  margin-left: 0.5rem;
}
```

## The HTML

```html
<label class="switch switch-md">
  <input type="checkbox" checked />
  <span class="switch-track"></span>
</label>
<span class="switch-label">Notifications</span>

<label class="switch switch-sm">
  <input type="checkbox" />
  <span class="switch-track"></span>
</label>
<span class="switch-label">Compact mode</span>
```

## What Happens

- `switch-tokens` emits `--switch-*` on `:root`; every track reads them with `var()`, so a theme swap is four values, not a search-and-replace.
- Each size rule derives height, knob size, the vertical inset, and the knob travel from one base number with `num()` — a single array drives the whole scale.
- The knob is the track's `::before`, so `:checked` only needs to change the background and push one transform.
- `:focus-visible` draws the ring on the track, keeping keyboard focus visible while the input stays hidden.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a `--busy` state that dims the track and shows the spinner from project 19 inside the knob.
2. Swap on/off colors per instance by overriding `--switch-on` inline.
3. Add a `switch-inline` label where the text sits before the track for right-to-left layouts.

## Summary

A hidden checkbox, a token root, and one array of base widths. Every dimension is a ratio, so the switch scales cleanly — and a theme only has to redefine four variables.

---

**Next: [21 — Image Gallery](../21-image-gallery.md)**
