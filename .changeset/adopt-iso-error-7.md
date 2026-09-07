---
"assertron": minor
---

Raise `iso-error` to `^7.0.0`.

`iso-error@6.0.5`'s caret range could never resolve to `iso-error@7.0.0`, so a fresh
install of `assertron` kept pulling a stale `iso-error` major even after `iso-error` 7
was published. This bump lets consumers actually resolve the current `iso-error` major
and removes the stale transitive `type-plus`/`tersify` copies that came with it.

`iso-error@7.0.0`'s major came from raising its own `engines.node` floor to `>= 20`
(required by `type-plus@8`'s `unpartial` dependency), not from an API change.
`assertron` already declares `engines.node: >= 20`, so nothing moves there. `assertron`
extends `iso-error`'s `ModuleError` in its public `AssertionError` type, but
`ModuleError`'s own shape is unchanged in `iso-error@7`, so this is a dependency range
update with no consumer-visible contract change — no source changes were required and
`pnpm verify` passes unmodified.
