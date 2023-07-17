# Improved Function Inference in Objects and Methods

## TL;DR

関数の引数の型推論が強化された。

## Prev

produceの返すT型はconsumeから推論可能だが、検出できなかった。

```typescript
declare function f<T>(arg: {
    produce: (n: string) => T,
    consume: (x: T) => void }
): void;


f({
    produce() { return "hello" },
    consume: x => x.toLowerCase(), // ❌ ERROR 
});
```

## Current

オブジェクト内の関数全体を評価してから推論するため、T型が検出可能になった。

```typescript
declare function f<T>(arg: {
    produce: (n: string) => T,
    consume: (x: T) => void }
): void;


f({
    produce() { return "hello" },
    consume: x => x.toLowerCase(), // (parameter) x:string
});
```
