# 38 — Stepper Wizard

A wizard is a sequence, and a sequence is what `count()` was made for. Build the dots and connectors once, derive progress with `num()`, and let an event describe `done`, `active`, and `upcoming`.

## The Brief

- A horizontal stepper of numbered dots joined by connectors.
- Step count and step classes generated from one array.
- `done`, `active`, and `upcoming` states per step.
- A progress bar whose width is computed from the current step index.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #94a3b8;
  line: #e2e8f0;
  primary: #2563eb;
  success: #10b981;
}

/* token root */
@define step-root(root: :root) {
  `
  @use(root) {
    --step-dot: 32px;
    --step-gap: 0.5rem;
    --step-radius: 999px;
  }
  `
}

@step-root()

@define stepper() {
  `
  .stepper {
    display: flex;
    align-items: flex-start;
    font-family: system-ui, sans-serif;
  }

  .step {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.4rem;
    position: relative;
  }

  .step-dot {
    width: var(--step-dot, 32px);
    height: var(--step-dot, 32px);
    border-radius: var(--step-radius, 999px);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.8rem;
    font-weight: 700;
    border: 2px solid @fun.color.line.value;
    background: #ffffff;
    color: @fun.color.muted.value;
    z-index: 1;
  }

  .step-label {
    font-size: 0.75rem;
    color: @fun.color.muted.value;
    text-align: center;
  }

  .step:not(:last-child)::after {
    content: "";
    position: absolute;
    top: calc(var(--step-dot, 32px) / 2);
    left: 50%;
    width: 100%;
    height: 2px;
    background: @fun.color.line.value;
    z-index: 0;
  }
  `
}

@stepper()
```

Step states are two lookups — the dot's fill and the label's ink:

```fscss
@event dot(state) {
  if state: done { return: #10b981; }
  el-if state: active { return: #2563eb; }
  el { return: #cbd5e1; }
}

@event ink(state) {
  if state: done { return: #10b981; }
  el-if state: active { return: #0f172a; }
  el { return: #94a3b8; }
}

.step--done .step-dot {
  border-color: @event.dot(done);
  background: @event.dot(done);
  color: #ffffff;
}
.step--done .step-label { color: @event.ink(done); }

.step--active .step-dot {
  border-color: @event.dot(active);
  color: @event.dot(active);
}
.step--active .step-label {
  color: @event.ink(active);
  font-weight: 600;
}
```

## The Math

Two numbers — where you are and how many steps there are — compute the fill:

```fscss
$total: 4;
$current: 2;

.step-progress {
  height: 4px;
  border-radius: 999px;
  background: @fun.color.line.value;
  overflow: hidden;
  margin-bottom: 1.5rem;
  max-width: 420px;
}

.step-progress-fill {
  height: 100%;
  width: num(<$current! - 1> / <$total! - 1> * 100)%;
  background: @fun.color.primary.value;
}
```

## The Data

The step classes come from one array. Add a fifth stage and the loop writes `.step-5`:

```fscss
@arr(steps[Cart, Address, Payment, Review])
@arr(si[count(@arr.steps!.length, 1)])

.step-@arr.si[] {
  order: num(@arr.si[]);
}
```

## The HTML

```html
<div class="step-progress">
  <div class="step-progress-fill"></div>
</div>

<div class="stepper">
  <div class="step step-1 step--done">
    <span class="step-dot">1</span>
    <span class="step-label">Cart</span>
  </div>
  <div class="step step-2 step--active">
    <span class="step-dot">2</span>
    <span class="step-label">Address</span>
  </div>
  <div class="step step-3">
    <span class="step-dot">3</span>
    <span class="step-label">Payment</span>
  </div>
  <div class="step step-4">
    <span class="step-dot">4</span>
    <span class="step-label">Review</span>
  </div>
</div>
```

## What Happens

- `stepper` is a block define: the connector lives on `.step:not(:last-child)::after`, which a declarations-only define could not scope.
- Step colours come from two events — dot fill and label ink — so a new state is one branch, not a new class family.
- `num(<$current - 1> / <$total - 1> * 100)%` computes progress from the same numbers a runtime would pass in.
- Steps are generated from `@arr.steps`; the `order` property keeps dot order tied to the data index.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a `.stepper--vertical` modifier that flips `flex-direction` and moves the connector to the side.
2. Stamp a check glyph into done dots with a `str()` block instead of the step number.
3. Tint the connector up to the active step using the `done` ink from the event.

## Summary

A stepper is order plus state. `count()` writes the steps, `@event` describes them, and `num()` turns the current index into a width. Four stages or fourteen, it is the same block define.

---

**Next: [39 — Rating Stars](../39-rating-stars.md)**
