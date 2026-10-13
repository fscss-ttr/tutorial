# Your First Project

Time to put the four beginner concepts together. We'll build a small profile card page: a token layer, a reusable card, a button, and a few shorthand wins — then compile it or run it straight from the browser.

## The Brief

- A centered card with an avatar placeholder, a name, a role, and a short bio.
- One primary button.
- A small color system driven by variables.
- No frameworks, no build tools required.

## Step 1 — The Stylesheet

Create `style.fscss`:

```fscss
/* 1. Tokens */
$primary: #2563eb;
$ink: #0f172a;
$muted: #64748b;
$bg: #f1f5f9;
$card-pad: 1.75rem;
$radius: 16px;

/* 2. Reusable pieces */
str(avatar-skin, "
  background: linear-gradient(135deg, #93c5fd, #7c3aed);
  color: white;
")

@define button(bg, color) {
  background: @use(bg);
  color: @use(color);
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 8px;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
}

@define card(bg: white, radius: 16px, pad: 1.75rem) {
  background: @use(bg);
  border-radius: @use(radius);
  padding: @use(pad);
  box-shadow: 0 10px 30px rgba(15, 23, 42, 0.08);
}

/* 3. Page */
body {
  background: $bg!;
  color: $ink!;
  font-family: system-ui, sans-serif;
  min-height: 100vh;
  margin: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.profile {
  @card()
  width: 320px;
  text-align: center;
}

.avatar {
  avatar-skin
  width: 80px;
  height: 80px;
  border-radius: 50%;
  margin: 0 auto 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.75rem;
  font-weight: 700;
}

.profile-name {
  margin: 0;
  font-size: 1.25rem;
}

.profile-role {
  color: $muted!;
  font-size: 0.9rem;
  margin: 0.25rem 0 1rem;
}

.profile-bio {
  color: $muted!;
  font-size: 0.9rem;
  line-height: 1.5;
  margin-bottom: 1.25rem;
}

.action {
  @button($primary, white)
  width: 100%;
}
```

A few things to notice:

- `$` variables carry the color system; change one and the whole card follows.
- `avatar-skin` is a stored block reused by writing its bare name.
- `@card()` and `@button(...)` are parameterized and composed.
- The avatar uses repeated sizing; try swapping in `%2(width, height[: 80px;])` as an exercise.

## Step 2 — The HTML

Create `index.html` next to it:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Profile — FSCSS</title>
  <link type="text/fscss" href="style.fscss">
  <script src="https://cdn.jsdelivr.net/npm/fscss@1.2.5/runtime.min.js" defer></script>
</head>
<body>
  <article class="profile">
    <div class="avatar">AS</div>
    <h1 class="profile-name">Ada Siri</h1>
    <p class="profile-role">Frontend Developer</p>
    <p class="profile-bio">Building fast, accessible interfaces with FSCSS.</p>
    <button class="action">Follow</button>
  </article>
</body>
</html>
```

Open the file. The runtime compiles `style.fscss` and styles appear.

## Step 3 — Compile It Instead

For a production-flavoured version, drop the runtime and compile ahead of time:

```bash
fscss style.fscss style.css
```

Then swap the `<head>` for a normal stylesheet link:

```html
<link rel="stylesheet" href="style.css">
```

Your page now ships plain CSS with no runtime.

## Exercises

1. Change `$primary` and watch every accent update on reload.
2. Convert the avatar's `width`/`height` pair to `%2(width, height[: 80px;])`.
3. Add a `.profile--compact` class that calls `@card(#0f172a, 16px)` for a dark variant.
4. Give `.action` a hover state that darkens the background using a second `$` variable.

## Summary

You used variables for the color system, `str()` for a stored block, `@define` for reusable components, and both the runtime and the CLI. That is the whole beginner toolkit. Everything from here is about doing these things at scale.

---

**Next: [Intermediate](../04-intermediate/README.md)**
