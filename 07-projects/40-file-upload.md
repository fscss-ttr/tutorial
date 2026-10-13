# 40 — File Upload

An uploader is a dropzone plus a queue of jobs. The dropzone is one block define, the queue rows are data, and every row's progress and status come from an array and an event.

## The Brief

- A dashed dropzone with a title, a hint, and type chips.
- A queue of file rows, each with a progress track.
- `queued`, `uploading`, `done`, and `error` states.
- Accepted types generated from one array.

## The FSCSS

```fscss
@fun(color){
  surface: #ffffff;
  bg: #f8fafc;
  ink: #0f172a;
  muted: #64748b;
  line: #cbd5e1;
  accent: #2563eb;
}

/* token root */
@define up-root(root: :root) {
  `
  @use(root) {
    --up-radius: 12px;
    --up-pad: 2rem;
    --up-track: #e2e8f0;
  }
  `
}

@up-root()

@define dropzone() {
  `
  .dropzone {
    border: 2px dashed @fun.color.line.value;
    border-radius: var(--up-radius, 12px);
    background: @fun.color.bg.value;
    padding: var(--up-pad, 2rem);
    text-align: center;
    font-family: system-ui, sans-serif;
    color: @fun.color.muted.value;
    max-width: 440px;
    transition: border-color 0.15s ease, background 0.15s ease;
  }

  .dropzone:hover {
    border-color: @fun.color.accent.value;
    background: #eff6ff;
  }

  .dropzone-title {
    color: @fun.color.ink.value;
    font-weight: 700;
    font-size: 0.95rem;
    margin-bottom: 0.25rem;
  }

  .dropzone-hint { font-size: 0.8rem; }

  .upload-list {
    max-width: 440px;
    margin-top: 1rem;
    display: flex;
    flex-direction: column;
    gap: 0.6rem;
    font-family: system-ui, sans-serif;
  }

  .file {
    display: grid;
    grid-template-columns: 1fr auto;
    gap: 0.25rem 0.75rem;
    align-items: center;
    background: @fun.color.surface.value;
    border: 1px solid @fun.color.line.value;
    border-radius: 8px;
    padding: 0.65rem 0.85rem;
  }

  .file-name {
    font-size: 0.85rem;
    font-weight: 600;
    color: @fun.color.ink.value;
  }

  .file-status { font-size: 0.75rem; }

  .file-track {
    grid-column: 1 / -1;
    height: 4px;
    border-radius: 999px;
    background: var(--up-track, #e2e8f0);
    overflow: hidden;
  }

  .file-fill {
    height: 100%;
    width: 0;
    background: @fun.color.accent.value;
    transition: width 0.3s ease;
  }
  `
}

@dropzone()
```

A row's colour depends on its state, so one event paints both the label and its bar:

```fscss
@event status(state) {
  if state: done { return: #10b981; }
  el-if state: uploading { return: #2563eb; }
  el-if state: error { return: #dc2626; }
  el { return: #64748b; }
}

.file--done .file-status { color: @event.status(done); }
.file--done .file-fill { background: @event.status(done); }

.file--uploading .file-status { color: @event.status(uploading); }
.file--uploading .file-fill { background: @event.status(uploading); }

.file--error .file-status { color: @event.status(error); }
.file--error .file-fill { background: @event.status(error); }

.file--queued .file-status { color: @event.status(queued); }
```

## The Data

The demo queue is an array of percentages walked once. Row `n` gets its width from `pct[n]`:

```fscss
@arr(files[a, b, c])
@arr(pct[100, 60, 0])
@arr(fi[count(@arr.files!.length, 1)])

.file-@arr.fi[] .file-fill {
  $idx: @arr.fi[];
  width: num(@arr.pct[$idx!] * 1)%;
}
```

The accepted types are the array that fills the dropzone footer:

```fscss
@arr(types[PNG, JPG, PDF, ZIP])
@arr(ti[count(@arr.types!.length, 1)])

.type-@arr.types[@arr.ti[]] {
  display: inline-block;
  margin: 0.75rem 0.15rem 0;
  font-size: 0.68rem;
  letter-spacing: 0.4px;
  padding: 0.15rem 0.45rem;
  border-radius: 4px;
  background: #e2e8f0;
  color: #475569;
}
```

## The HTML

```html
<div class="dropzone">
  <div class="dropzone-title">Drop files here</div>
  <div class="dropzone-hint">or click to browse — up to 25 MB</div>
  <span class="type-PNG">PNG</span>
  <span class="type-JPG">JPG</span>
  <span class="type-PDF">PDF</span>
  <span class="type-ZIP">ZIP</span>
</div>

<div class="upload-list">
  <div class="file file-1 file--done">
    <span class="file-name">report.pdf</span>
    <span class="file-status">Done</span>
    <span class="file-track"><span class="file-fill"></span></span>
  </div>

  <div class="file file-2 file--uploading">
    <span class="file-name">hero.png</span>
    <span class="file-status">Uploading</span>
    <span class="file-track"><span class="file-fill"></span></span>
  </div>

  <div class="file file-3 file--queued">
    <span class="file-name">archive.zip</span>
    <span class="file-status">Queued</span>
    <span class="file-track"><span class="file-fill"></span></span>
  </div>
</div>
```

## What Happens

- `dropzone` is one block define: the dashed surface, the hover swap, the queue grid, and the track all sit together.
- `.file-fill` widths come from the `pct` array through a single `count()` loop, so the demo queue is data, not copy-paste.
- `@event status` maps a named state to a colour used by both the label and the bar, so the two never drift.
- The radius, padding, and track colour are `--up-*` tokens from the root define; the widget re-skins from three values.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a `.dropzone--active` class for the drag-over state and share its border with the hover rule.
2. Colour each accepted type from a parallel array instead of the single grey chip.
3. Add a per-row remove button with `%2(width, height[: 20px;])` and a `str()` glyph.

## Summary

A dropzone block define, a queue array, and one status event. The file list is data, the progress is arithmetic, and the four states are a single lookup — the uploader stays the same whether it shows three files or thirty.

---

**Next: [41 — Comment Thread](../41-comment-thread.md)**
