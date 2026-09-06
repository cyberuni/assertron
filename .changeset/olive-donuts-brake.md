---
'assertron': minor
---

Move `type-plus` from `^7.6.2` to `^8.0.0-beta.10`.

`type-plus` is a runtime `dependencies` entry and its types leak into assertron's
public declarations (`RequiredPick` in `errors.d.ts`, `AnyConstructor` in
`assertron.d.ts`, and `If`/`IsExtend`/`NonComposableTypes` in `satisfies.d.ts`),
so consumers resolve and compile against the new major. That is a
consumer-visible change to what gets installed, hence a minor rather than a
patch. Assertron's own exported signatures are unchanged and no source edits
were needed — every type it uses exists in v8 with the same meaning — so it is
not a major.

The motivating benefit is deduplication: `type-plus` 7 depends on `tersify` ^3
while assertron already depends on `tersify` ^4 directly. Pairing assertron with
`type-plus` 8 (which itself depends on `tersify` ^4.0.6) removes the second
`tersify` major from downstream dependency trees.

The caret range resolves forward to a stable `8.0.0` when it ships.
