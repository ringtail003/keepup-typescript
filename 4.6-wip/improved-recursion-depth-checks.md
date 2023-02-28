# Improved Recursion Depth Checks

## TL;DR

ネストした型のチェックスピードが改善した。

## 前提

ジェネリクスを持ったインターフェースで考える。

```typescript
interface Foo<T> {
  prop: T;
}
```

Tの型が一致すれば互換性がある。

```typescript
// OK
let x1: Foo<string> = { prop: "" };
let y1: Foo<string> = { prop: "" };
x1 = y1;

// ERROR
let x2: Foo<string> = { prop: "" };
let y2: Foo<number> = { prop: 0 };
x2 = y2;
```

ジェネリクスがネストしている場合も同じ、Tの型が一致すれば互換性がある。

```typescript
// OK
let x4: Foo<Foo<string>> = { prop: { prop: "" } };
let y4: Foo<Foo<string>> = { prop: { prop: "" } };
x4 = y4;

// ERROR
let x1: Foo<Foo<string>> = { prop: { prop: "" } };
let y1: Foo<Foo<number>> = { prop: { prop: 0 } };
x1 = y1;
```

## 変更点

深いネストの型チェックの方法が見直され、スピードが改善した。

```typescript
let x: Foo<Foo<Foo<Foo<Foo<Foo<string>>>>>>;
let y: Foo<Foo<Foo<Foo<Foo<string>>>>>;

// ERROR
// エラーになること自体は以前のバージョンと4.6どちらでも変わらない
x = y;
```
