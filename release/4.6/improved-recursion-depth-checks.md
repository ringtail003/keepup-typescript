# Improved Recursion Depth Checks

## TL;DR

ネストした型のチェックスピードが改善した。

## 前提

ジェネリクスは、後から決定する型をプレースホルダのように扱える。

```typescript
interface Foo<T> {
  prop: T;
}

let x1: Foo<string>; // x1: { prop: string }
let x2: Foo<number>; // x2: { prop: number }
```

ネストした型も扱うことができる。

```typescript
let x3: Foo<Foo<string>>; // x3: { prop: { prop: string } }
```

ネストが深すぎるとコンパイル時にパフォーマンスの問題が発生する。

```typescript
declare const x: Foo<Foo<Foo<Foo<Foo<string>>>>>;
declare const y: Foo<Foo<Foo<Foo<Foo<Foo<string>>>>>>;

// パッと見てネストの深さが違うため「互換性がない」ことが分かるが、機械的なチェックではネストを遡って検査する
```

## Previous

深くネストしている場合、型の互換性の推論が不完全だった。

```typescript
declare const x: Foo<Foo<Foo<Foo<Foo<string>>>>>;
declare const y: Foo<Foo<Foo<Foo<Foo<Foo<string>>>>>>;

x = y; // ❓ 互換性はないのに、エラーにならない
```

## Current

型チェックが見直され、推論が正しく行われるようになりパフォーマンスが改善した。

```typescript
declare const x: Foo<Foo<Foo<Foo<Foo<string>>>>>;
declare const y: Foo<Foo<Foo<Foo<Foo<Foo<string>>>>>>;

x = y; // エラーが発生する
```
