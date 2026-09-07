# assertron

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
