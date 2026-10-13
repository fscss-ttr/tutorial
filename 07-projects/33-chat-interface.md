# 33 — Chat Interface

A chat window is a layout problem and a data problem: a scrolled thread, a header, a composer. Build it with a scoped block define for the shell, array-generated message rows, an `@event` that decides who speaks, and a `rpt()` typing indicator.

## The Brief

- A shell block define owns the header, thread, and composer, with a phone breakpoint.
- Message rows generated from an array; `.msg--me` flips alignment and re-calls the speaker `@event`s.
- One `@event` pair chooses bubble background *and* ink for either side.
- A typing row whose dots are a `rpt(3, '·')` content string, animated by a compact keyframes macro.
- Vendor-prefixed `backdrop-filter` on the header and `user-select` on avatars.

## The FSCSS

```fscss
/* tokens */
@define chat-tokens(root: :root) {
  `
  @use(root){
    --chat-bg: #f4f6fb;
    --chat-surface: #eef1f7;
    --chat-me: #2563eb;
    --chat-ink: #0f172a;
    --chat-muted: #64748b;
    --chat-radius: 16px;
  }
  `
}

@chat-tokens()

/* who speaks: tone and ink, one event each, two call sites */
@event bubble-tone(side) {
  if side: me {
    return: var(--chat-me, #2563eb);
  }
  el {
    return: var(--chat-surface, #eef1f7);
  }
}

@event bubble-ink(side) {
  if side: me {
    return: #ffffff;
  }
  el {
    return: var(--chat-ink, #0f172a);
  }
}

/* the thread: four rows from one walk */
@arr(msgs[dana, sam, lena, jamie])
@arr(mi[count(4, 1)])

.msg-@arr.msgs[@arr.mi[]] {
  display: flex;
  align-items: flex-end;
  gap: 0.5rem;
  max-width: 85%;
  margin-bottom: 0.9rem;
}

.msg-@arr.msgs[@arr.mi[]].msg--me {
  align-self: flex-end;
  flex-direction: row-reverse;
}

.msg .msg-av {
  size: 30px;
  border-radius: 50%;
  display: grid;
  place-items: center;
  font-size: 0.7rem;
  font-weight: 700;
  background: #dbeafe;
  color: var(--chat-me, #2563eb);
  -*-user-select: none;
}

.msg .msg-bubble {
  padding: 0.6rem 0.85rem;
  border-radius: var(--chat-radius, 16px);
  background: @event.bubble-tone('them');
  color: @event.bubble-ink('them');
}

.msg--me .msg-bubble {
  background: @event.bubble-tone('me');
  color: @event.bubble-ink('me');
}

.msg .msg-time {
  font-size: 0.66rem;
  color: var(--chat-muted, #64748b);
  margin-top: 0.3rem;
}

.msg--me .msg-time { text-align: right; }

/* the shell: one scoped block define, breakpoint included */
@define chat-shell(st: .chat) {
  `
  @use(st){
    display: flex;
    flex-direction: column;
    height: 560px;
    max-width: 420px;
    margin: 2rem auto;
    border-radius: 20px;
    overflow: hidden;
    background: var(--chat-bg, #f4f6fb);
    box-shadow: 0 24px 60px rgba(15, 23, 42, 0.14);
  }

  @use(st) .chat-header {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    padding: 0.9rem 1.1rem;
    border-bottom: 1px solid rgba(15, 23, 42, 0.06);
    -*-backdrop-filter: blur(12px);
  }

  @use(st) .chat-name { font-weight: 700; color: var(--chat-ink, #0f172a); }
  @use(st) .chat-status { font-size: 0.72rem; color: var(--chat-muted, #64748b); }

  @use(st) .chat-thread {
    display: flex;
    flex-direction: column;
    flex: 1;
    overflow-y: auto;
    padding: 1.25rem;
  }

  @use(st) .chat-composer {
    display: flex;
    gap: 0.5rem;
    padding: 0.85rem;
    min-height: num(4 * 14)px;
    border-top: 1px solid rgba(15, 23, 42, 0.06);
    background: var(--chat-surface, #eef1f7);
  }

  @use(st) .chat-input {
    flex: 1;
    resize: none;
    border: none;
    border-radius: 10px;
    padding: 0.6rem 0.8rem;
    font: inherit;
    background: #ffffff;
  }

  @use(st) .chat-send {
    %2(width, height[: 40px;])
    border: none;
    border-radius: 50%;
    align-self: flex-end;
    background: var(--chat-me, #2563eb);
    color: #ffffff;
    cursor: pointer;
  }

  @use(st) .chat-send:disabled {
    opacity: 0.45;
    cursor: not-allowed;
  }

  @media (max-width: 480px) {
    @use(st){
      height: 100vh;
      max-width: none;
      border-radius: 0;
      margin: 0;
    }
  }
  `
}

@chat-shell()

/* the typing indicator: dots via rpt(), motion via the compact macro */
.msg-typing {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.msg-typing .msg-bubble {
  padding: 0.75rem 1rem;
  width: fit-content;
}

.msg-typing .msg-bubble::after {
  content: "rpt(3, '·')";
  letter-spacing: 2px;
  animation: typing-blink 1.2s steps(4) infinite;
}

$(@keyframes typing-blink, .msg-typing .msg-bubble::after, &[1.2s steps(4) infinite]) {
  0%, 100% { opacity: 0.25; }
  50% { opacity: 1; }
}
```

## The HTML

```html
<section class="chat">
  <header class="chat-header">
    <span class="msg-av">JT</span>
    <div>
      <div class="chat-name">Jordan Tate</div>
      <div class="chat-status">online</div>
    </div>
  </header>

  <div class="chat-thread">
    <div class="msg-dana">
      <span class="msg-av">DT</span>
      <div>
        <div class="msg-bubble">Good morning! Shipment landed?</div>
        <div class="msg-time">13:02</div>
      </div>
    </div>

    <div class="msg-sam msg--me">
      <span class="msg-av">SM</span>
      <div>
        <div class="msg-bubble">Dropped off in one piece.</div>
        <div class="msg-time">13:05</div>
      </div>
    </div>

    <div class="msg-lena">
      <span class="msg-av">LT</span>
      <div>
        <div class="msg-bubble">Invoice is in your inbox — mind the totals.</div>
        <div class="msg-time">13:09</div>
      </div>
    </div>

    <div class="msg-jamie msg--me">
      <span class="msg-av">JW</span>
      <div>
        <div class="msg-bubble">On it. Will review by close.</div>
        <div class="msg-time">13:14</div>
      </div>
    </div>

    <div class="msg-typing">
      <span class="msg-av">JT</span>
      <div class="msg-bubble"></div>
    </div>
  </div>

  <form class="chat-composer">
    <textarea class="chat-input" rows="1" placeholder="Message…"></textarea>
    <button class="chat-send" type="submit">→</button>
  </form>
</section>
```

## What Happens

- The four rows come from one array walk; `.msg--me` in the markup flips the row *and* re-calls both `@event`s with `'me'`, so speaker styling is two arguments, not two rule sets.
- The shell block define scopes every rule under `@use(st)` via backticks, computes the composer height with `num(4 * 14)px`, and folds the whole thing to a phone frame in its own breakpoint.
- `rpt(3, '·')` builds the typing dots inside the content string; the compact keyframes macro attaches the blink to the `::after` in one declaration.
- `-*-backdrop-filter` and `-*-user-select` expand to their vendor families.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Drive whose turn it is from a `@arr(sides[them, me, them, me])` so the walk also emits alignment.
2. Fold the two bubble `@event`s into one `@define msg-skin(side)` that sets background *and* color.
3. Add date-divider chips generated from a `@arr(days[Today, Yesterday, Monday])` walk.

## Summary

A threaded list, an evented speaker, a scoped shell, and a `rpt()` typing row: the whole chat window reduces to data and two decisions. Conversation, minus the copy-paste.

---

**Next: [34 — Music Player](../34-music-player.md)**