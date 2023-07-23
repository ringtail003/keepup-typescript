# Improved Inference from Binding Patterns

## TL;DR

引数から導出する戻り値の型推論が改善された。

## Prev

以下の例では引数の値から、分割代入する変数の型が推論される。

```typescript
declare const fn: <T,U>(x:T, y:U) => T;
const [a, b, c] = fn([0, 'a', true], [1, 'b', false]);
// a:number
// b:string
// c:boolean
```

引数がオプショナルな時、引数を省略すると分割代入は失敗するが推論上エラーにならなかった。

```typescript
declare const fn2: <T>(x?: T) => T;
const [d, e, f] = fn2();
```

## Current

4.8で改善されエラーが得られるようになった。

```typescript
declare const fn2: <T>(x?: T) => T;
const [d, e, f] = fn2(); // ❌
```
