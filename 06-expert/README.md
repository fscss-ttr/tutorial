# 06 — Expert

Composition and performance.

The final climb. Experienced FSCSS means you design *modules other people import*, you understand the two runtime entry points and when each belongs, you ship production output that carries no runtime, and you understand enough of the internals to contribute back.

## What You'll Learn

| Lesson | Topic | Description |
|--------|-------|-------------|
| [Module Design](./module-design.md) | Architecture | Build a clean, importable FSCSS module |
| [Registry Imports](./registry-imports.md) | `fscss:name` | Pull libraries from the FSCSS registry (1.2.1+) |
| [runtime.js vs esm.js](./runtime-vs-esm-deep.md) | Runtime | Auto-scan vs manual `reboot()` |
| [CLI Production Output](./cli-production.md) | Build | Compile a deployable stylesheet, no runtime |
| [Internals & Contributing](./internals-and-contributing.md) | Community | How the compiler works, and how to help |

## Goal

Ship a tiny module others can `@import` cleanly.

---

**Start: [Module Design](./module-design.md)**

---

*Written by [Omonire](https://github.com/Omonire)*