---
"assertron": patch
---

Pin `type-plus` to the exact version `8.0.0-beta.10`.

`assertron` already depended on `type-plus@^8.0.0-beta.10`, so this only tightens the
caret range to an exact pin — it does not change which version a fresh install resolves
today. The pin forecloses future resolution to a later `8.0.0-beta.x`, `8.0.0`, or
`8.1.0` release, but nothing consumer-visible changes right now: no source changed, the
`typescript >= 5.6.0` peer requirement was already reached through `type-plus` before
this change, and `path-equal` was refreshed to its latest patch (`^1.2.8`) with no
functional impact.
