# Tail-Recursion Elimination on Conditional Types

Conditional Typesの末尾再帰の削除が実装された。

## TL;DR

以前のバージョンでは型の再帰が深いとエラーになっていた。

```typescript
type TrimLeft<T extends string> =
    T extends ` ${infer Rest}`
      ? TrimLeft<Rest> // <=== 自分自身を呼び出す再帰
      : T;
    
// Error: Type instantiation is excessively deep and possibly infinite.
type Test = 
  TrimLeft<"                                                oops">;
```

TS4.5で末尾再帰の最適化が導入され、エラーにならなくなった。

```typescript
type TrimLeft<T extends string> =
    T extends ` ${infer Rest}`
      ? TrimLeft<Rest>
      : T;
    
// OK: "oops"
type Test = 
  TrimLeft<"                                                oops">;
```

ただし結果をUnion Typeに保存する場合は最適化が適用されませんよ、という話。

```typescript
type GetChars<S> =
    S extends `${infer Char}${infer Rest}`
        ? Char | GetChars<Rest> // <=== Union Type
        : never;

// Error Type instantiation is excessively deep and possibly infinite.
type Chars = GetChars<"A1234567890A1234567890A1234567890A1234567890A1234">;
```

## 型の再帰

一般的なプログラミングで自分自身を呼び出す「再帰」という手法がある。

```typescript
function fn(n) {
  if (n === 0) {
    return n;
  }
  return fn(n - 1);
}
```

再帰が必要なユースケースとしては、どこまでネストが続くか分からないもの。ツリー上になったDOM構造や、中に何が入っているのか分からないPromiseやZipファイルなんかにも当てはめて考えられる。

```typescript
function unzip(file) {
  if (file.type !== "zip") {
    return file;
  }
  return unzip(file);
}
```

関数の一番最後で再帰することを「末尾再帰」と呼ぶ。

```typescript
// 末尾再帰
function fn(n) {
  ...
  return fn(n - 1);
}

// 末尾再帰ではない
function fn(n) {
  ...
  const n2 = fn(n - 1);
  return n2;
}
```
