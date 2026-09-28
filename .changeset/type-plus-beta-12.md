---
"assertron": patch
---

Update `type-plus` to `8.0.0-beta.12`, `satisfier` to `^5.4.6`, and `tersify` to `^4.0.8`.

`SatisfyExpectation` no longer uses `If` and `IsExtend` from `type-plus`, because beta.12 changed `If` and removed `IsExtend`. It now uses plain conditional types with the same behavior.
