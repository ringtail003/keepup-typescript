# Narrowing On Comparisons to Booleans

## TL;DR

type guard function（型ガード関数）をBoolean比較した時、Narrowingが適用されるようになった。

## use-cases

```typescript
declare function isString(x: unknown): x is string;

if (isString(x)) {
    // x: string
}

if (isString(x) === true) {
    // [v5.2] x: unknown
    // [v5.3] x: string
}
```
