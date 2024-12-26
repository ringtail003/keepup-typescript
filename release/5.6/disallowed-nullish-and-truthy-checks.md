# Disallowed Nullish and Truthy Checks

## TL;DR

.必ずtrue・falseのどちらかになるif文に対して、エラーが検出されるようになった。

```typescript
// .test()を忘れている
if (/0x[0-9a-f]/) { ... }

// "<="と書こうとして間違えた
if (x => 0) { ... }

// "(v1 < v2) ?? 100"と解釈される
return v1 < v2 ?? 100;
```
