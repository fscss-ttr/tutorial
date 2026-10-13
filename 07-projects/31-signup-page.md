# 31 — Signup Page

A signup flow adds password confirmation, terms acceptance, and a couple more fields. Reuse the login tokens/defines where sensible, add a password strength hint driven by an `@event`, and keep everything centered and accessible.

## The Brief

- A centered auth layout reusing radius/max-width tokens.
- Fields: name, email, password, confirm password, and terms checkbox.
- Inline password strength hint gated by an `@event` (weak/ok/strong).
- Primary submit button with a loading intent and consistent focus/error rings.

## The FSCSS

```fscss
@fun(color){
  bg: radial-gradient(1200px 800px at 50% 0%, #f8fafc 0%, #e5e7eb 100%);
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  border: #cbd5e1;
  focus: #2563eb;
  danger: #dc2626;
  success: #10b981;
  ring: rgba(37, 99, 235, 0.15);
  danger-ring: rgba(220, 38, 38, 0.12);
  success-ring: rgba(16, 185, 129, 0.14);
}

@fun(space){
  pad: 2rem;
}

@define signup-root() {
  `
  :root {
    --auth-radius: 14px;
    --max-w: 460px;
  }
  `
}

@define signup-shell() {
  `
  body {
    margin: 0;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: @fun.color.bg.value;
    font-family: system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
    color: @fun.color.ink.value;
    padding: 2rem 1rem;
  }

  .auth {
    width: min(94vw, var(--max-w, 460px));
    display: flex;
    flex-direction: column;
    gap: 1.25rem;
  }

  .auth-brand {
    text-align: center;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .auth-title {
    font-size: clamp(1.6rem, 4vw, 2.4rem);
    font-weight: 700;
    letter-spacing: -0.02em;
  }

  .auth-tagline {
    color: @fun.color.muted.value;
  }

  .auth-card {
    background: @fun.color.surface.value;
    border: 1px solid rgba(15, 23, 42, 0.06);
    border-radius: var(--auth-radius, 14px);
    padding: clamp(1.25rem, 4vw, @fun.space.pad.value);
    box-shadow: 0 24px 80px rgba(15, 23, 42, 0.14);
    display: flex;
    flex-direction: column;
    gap: 1.25rem;
  }

  .auth-footer {
    text-align: center;
    color: @fun.color.muted.value;
    font-size: 0.95rem;
  }

  .auth-link {
    color: @fun.color.focus.value;
    text-decoration: none;
    font-weight: 600;
  }
  `
}

@define signup-form() {
  `
  .form {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .grid-2 {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1rem;
  }

  .field {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .label {
    font-size: 0.92rem;
    font-weight: 600;
  }

  .input {
    width: 100%;
    padding: 0.75rem 0.9rem;
    border-radius: calc(var(--auth-radius, 14px) - 4px);
    border: 1px solid @fun.color.border.value;
    background: @fun.color.surface.value;
    color: @fun.color.ink.value;
    font-size: 1rem;
    transition: border-color 0.15s ease, box-shadow 0.15s ease;
    outline: none;
  }

  .input::placeholder {
    color: @fun.color.muted.value;
  }

  .input:focus {
    border-color: @fun.color.focus.value;
    box-shadow: 0 0 0 3px @fun.color.ring.value;
  }

  .help {
    color: @fun.color.muted.value;
    font-size: 0.88rem;
  }

  .btn-primary {
    border: none;
    border-radius: calc(var(--auth-radius, 14px) - 4px);
    padding: 0.85rem 1rem;
    font-size: 1rem;
    font-weight: 700;
    cursor: pointer;
    background: @fun.color.focus.value;
    color: @fun.color.surface.value;
    transition: filter 0.15s ease, transform 0.05s ease;
  }

  .btn-primary:hover { filter: brightness(1.05); }
  .btn-primary:active { transform: translateY(1px); }
  .btn-primary:disabled { opacity: 0.55; cursor: not-allowed; }

  @media (max-width: 520px) {
    .grid-2 { grid-template-columns: 1fr; }
  }
  `
}

@define signup-strength() {
  `
  .strength {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: 0.88rem;
    color: @fun.color.muted.value;
  }

  .strength-bar {
    flex: 1;
    height: 6px;
    border-radius: 999px;
    background: rgba(15, 23, 42, 0.08);
    overflow: hidden;
  }

  .strength-fill {
    height: 100%;
    width: 0%;
    background: @fun.color.muted.value;
    transition: width 0.2s ease, background-color 0.2s ease;
  }

  .strength.is-weak .strength-fill {
    width: 33%;
    background: @fun.color.danger.value;
  }

  .strength.is-ok .strength-fill {
    width: 66%;
    background: @fun.color.focus.value;
  }

  .strength.is-strong .strength-fill {
    width: 100%;
    background: @fun.color.success.value;
  }

  .strength.is-weak .strength-text { color: @fun.color.danger.value; }
  .strength.is-ok .strength-text { color: @fun.color.focus.value; }
  .strength.is-strong .strength-text { color: @fun.color.success.value; }
  `
}

@event pw-strength(score: 0) {
  if score >= 80 {
    return: "strong";
  }
  el-if score >= 40 {
    return: "ok";
  }
  el-if score >= 1 {
    return: "weak";
  }
  el {
    return: "none";
  }
}

@signup-root()
@signup-shell()
@signup-form()
@signup-strength()
```

## The HTML

```html
<main class="auth">
  <header class="auth-brand">
    <h1 class="auth-title">Create account</h1>
    <p class="auth-tagline">Start your free workspace in seconds</p>
  </header>
  <section class="auth-card">
    <form class="form">
      <div class="grid-2">
        <div class="field">
          <label class="label" for="first">First name</label>
          <input class="input" id="first" placeholder="Jane">
        </div>
        <div class="field">
          <label class="label" for="last">Last name</label>
          <input class="input" id="last" placeholder="Doe">
        </div>
      </div>
      <div class="field">
        <label class="label" for="email">Email</label>
        <input class="input" id="email" type="email" placeholder="you@example.com">
      </div>
      <div class="field">
        <label class="label" for="pw">Password</label>
        <input class="input" id="pw" type="password" placeholder="Create a password">
        <div class="strength is-weak">
          <div class="strength-bar">
            <div class="strength-fill"></div>
          </div>
          <span class="strength-text">Weak</span>
        </div>
        <p class="help">Use 8+ chars with a mix of letters and numbers.</p>
      </div>
      <div class="field">
        <label class="label" for="confirm">Confirm password</label>
        <input class="input" id="confirm" type="password" placeholder="Re-enter password">
      </div>
      <label class="checkbox">
        <input type="checkbox">
        <span>I agree to the Terms and Privacy Policy</span>
      </label>
      <button class="btn-primary" type="submit">Create account</button>
    </form>
  </section>
  <footer class="auth-footer">
    Already have an account? <a class="auth-link" href="#">Sign in</a>
  </footer>
</main>
```

## What Happens

- `signup-root()` shares geometry (`--auth-radius`, `--max-w`) with the login flow; only the max width nudges up slightly for the extra fields.
- Two-column grid for name splits responsively at 520px (co-located media query inside `signup-form()`).
- `@event pw-strength(score)` maps a numeric score to `strong/ok/weak/none`; `.strength.is-weak/.is-ok/.is-strong` drives bar width and text color from tokens.
- Success/danger/focus rings are tokenized; the strength states reuse `success/focus/danger` to avoid magic colors.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Tie `pw-strength` to live scoring heuristics in the call site (length + classes) and reflect `.is-invalid` on confirm when mismatched.
2. Add a `.terms` row and `.help.is-error` if unchecked on submit intent.
3. Extract shared auth pieces (shell/card/brand/footer) into `str()` blocks and reuse between 30 and 31 to avoid duplication.

## Summary

Signup mirrors login's discipline: shared tokens, compact block defines, event-gated strength states, and a small responsive grid — more fields, same rules.

---

**Next: [32 — Profile Card](../32-profile-card.md)**