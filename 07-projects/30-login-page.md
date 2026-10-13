# 30 — Login Page

A login page is mostly form + layout + states. Use a token root, block defines for the shell and form, and an `@event` to gate inline validation (email/password) without polluting classes with logic.

## The Brief

- A centered auth layout with a brand, tagline, login form, and a footer link.
- Email/password inputs with labels, helper text, and an inline error state.
- Primary button, "remember me", and "forgot password" link.
- Use `@event` for validation state (neutral/invalid) and reflect it via CSS.

## The FSCSS

```fscss
@fun(color){
  bg: radial-gradient(1200px 800px at 50% 0%, #f1f5f9 0%, #e2e8f0 100%);
  surface: #ffffff;
  ink: #0f172a;
  muted: #64748b;
  border: #cbd5e1;
  focus: #2563eb;
  danger: #dc2626;
  ring: rgba(37, 99, 235, 0.15);
  danger-ring: rgba(220, 38, 38, 0.12);
}

@fun(space){
  pad: 2rem;
  gap: 1rem;
}

@define login-root() {
  `
  :root {
    --auth-radius: 14px;
    --max-w: 420px;
  }
  `
}

@define login-shell() {
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
  }

  .auth {
    width: min(92vw, var(--max-w, 420px));
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
    box-shadow: 0 24px 80px rgba(15, 23, 42, 0.16);
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

@define login-form() {
  `
  .form {
    display: flex;
    flex-direction: column;
    gap: 1.25rem;
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

  .row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    flex-wrap: wrap;
  }

  .checkbox {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    color: @fun.color.muted.value;
    font-size: 0.95rem;
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
  `
}

@define login-error() {
  `
  .input.is-invalid {
    border-color: @fun.color.danger.value;
    box-shadow: 0 0 0 3px @fun.color.danger-ring.value;
  }

  .help.is-error {
    color: @fun.color.danger.value;
  }
  `
}

@event auth-state(valid: 1) {
  if valid == 1 {
    return: "valid";
  }
  el-if valid == 0 {
    return: "invalid";
  }
  el {
    return: "neutral";
  }
}

@login-root()
@login-shell()
@login-form()
@login-error()
```

## The HTML

```html
<main class="auth">
  <header class="auth-brand">
    <h1 class="auth-title">Welcome back</h1>
    <p class="auth-tagline">Sign in to continue to your workspace</p>
  </header>
  <section class="auth-card">
    <form class="form">
      <div class="field">
        <label class="label" for="email">Email</label>
        <input class="input" id="email" type="email" placeholder="you@example.com" autocomplete="email">
        <p class="help">We'll never share your email.</p>
      </div>
      <div class="field">
        <label class="label" for="password">Password</label>
        <input class="input" id="password" type="password" placeholder="••••••••" autocomplete="current-password">
        <p class="help">At least 8 characters.</p>
      </div>
      <div class="row">
        <label class="checkbox">
          <input type="checkbox">
          <span>Remember me</span>
        </label>
        <a class="auth-link" href="#">Forgot password?</a>
      </div>
      <button class="btn-primary" type="submit">Sign in</button>
    </form>
  </section>
  <footer class="auth-footer">
    No account yet? <a class="auth-link" href="#">Create one</a>
  </footer>
</main>
```

## What Happens

- `login-root()` shares `--auth-radius` and `--max-w` across shell, card, inputs, and button (calc keeps corners consistent).
- `login-shell()` centers the auth card with flexbox and applies a soft radial background via token; `clamp()` keeps title and padding fluid.
- `@event auth-state()` is the gating point for validation modes (neutral/invalid/valid) — `.input.is-invalid` and `.help.is-error` reflect the invalid case.
- Focus rings are tokenized (`ring`, `danger-ring`) so error and focus never clash.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Use `@event.auth-state(0)` to conditionally style the submit button (disabled/aria-invalid) intent without inline JS assumptions.
2. Add a `.is-loading` state on `.btn-primary` via a block define (spinner + disabled).
3. Split the card into `str()` blocks (brand, form, footer) and compose via bare labels to see string composition in action.

## Summary

Centering + tokenized radius + event-gated validation makes a login page clean: states are explicit, geometry is shared, and the form reads the same way across breakpoints.

---

**Next: [31 — Signup Page](../31-signup-page.md)**