# assertron

## 11.7.0

### Minor Changes

- 7dd509c: Raise `iso-error` to `^7.0.0`.
  
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

## 11.6.1

### Patch Changes

- d5c0ed4: Pin `type-plus` to the exact version `8.0.0-beta.10`.
  
  `assertron` already depended on `type-plus@^8.0.0-beta.10`, so this only tightens the
  caret range to an exact pin — it does not change which version a fresh install resolves
  today. The pin forecloses future resolution to a later `8.0.0-beta.x`, `8.0.0`, or
  `8.1.0` release, but nothing consumer-visible changes right now: no source changed, the
  `typescript >= 5.6.0` peer requirement was already reached through `type-plus` before
  this change, and `path-equal` was refreshed to its latest patch (`^1.2.8`) with no
  functional impact.

## 11.6.0

### Minor Changes

- d5a9ba4: Move `type-plus` from `^7.6.2` to `^8.0.0-beta.10`.
  
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

## 11.5.5

### Patch Changes

- 419f686: Drop the last Node builtin from the shipped code.
  
  `AssertOrder`'s clock imported the bare `perf_hooks` specifier and preferred
  `process.hrtime`. It now uses `performance.now()`, which every runtime this package
  supports provides as a global. The published `esm/` and `cjs/` output no longer references
  any Node builtin, so it loads unchanged on Bun, Deno, browsers and edge runtimes.
  
  Elapsed times from `AssertOrder#end()` and `getTimeTaken()` are still high-resolution
  milliseconds; only the clock behind them changed. The `browser` field's
  `"perf_hooks": false` mapping is removed because there is no longer an import for a
  bundler to stub.
  
  `node:assert` is unchanged and unaffected: it appears only in the test helpers and spec
  files, neither of which is published.

## 11.5.4

### Patch Changes

- cdcd018: Raise the minimum of every runtime dependency to the version this release is built and
  tested against: `iso-error@6.0.5`, `path-equal@1.2.7`, `satisfier@5.4.4`, `tersify@4.0.6`
  and `type-plus@7.6.2`. Consumers resolve the newer upstreams as a result.
  
  The published bundles are also rebuilt by tsdown rather than three `tsc` passes. The public
  API is unchanged and `esm/index.js`, `cjs/index.js` and the declarations beside them keep
  their paths, but the emitted output differs: the CJS target moves from ES5 to ES2015
  (rolldown's floor), a small `_virtual/` helper module set appears alongside the entry, and
  per-module `.d.ts` files that were never reachable through the `exports` map are no longer
  emitted. Two source files that nothing referenced — `ts/assert-order/internalInterfaces.ts`
  and `ts/testUtils.ts` — are dropped from the published `ts/` sources.

## 11.5.3

### Patch Changes

- 363640b: Point repository metadata at `cyberuni/assertron` and release through npm trusted
  publishing (OIDC) instead of a long-lived `NPM_TOKEN`.

## 11.5.2

### Patch Changes

- 7b46ba0: Fix process is not defined in browser

## 11.5.1

### Patch Changes

- 49aaed5: fix global is not defined in browser.

## 11.5.0

### Minor Changes

- 358b239: Add message support for `true|false|truthy|falsy`

## 11.4.0

### Minor Changes

- cf66a04: Add `uuid()`
