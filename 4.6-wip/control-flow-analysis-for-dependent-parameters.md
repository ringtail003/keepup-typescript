# Control Flow Analysis for Dependent Parameters

## TL;DR

関数の引数で、タグ付きユニオンと同じことがタプルでもできるようになった。

## 前提

タグ付きユニオンとは、特定のプロパティの値により他のプロパティの型を推論する仕組み。

```typescript
const fn = (obj: { kind: 'a', payload: number } | { kind: 'b', payload: string }) => {
  // kind が 'a' ならば payload は number である
  if (obj.kind === 'a') {
    obj.payload.toFixed();
  }
  
  // kind が 'b' ならば payload は string である
  if (obj.kind === 'b') {
    obj.payload.concat('b');
  }
}
```

タプルとは、特定の順番で並んだ型。

```typescript
// タプルを宣言
type Tuple = [string, number, boolean];

// 順番どおり
const t1: Tuple = ['a', 1, true];

// ❌ ERROR! 
const t2: Tuple = ['a', true, 1];
```

## お題

ユーザーの入力を受け取ってsearch関数にわたすロジックを実装せよ。\
ユーザーの入力は `id（数値）`または `name（文字列）` のどちらかとする。

```typescript
declare const search: (params: { id: number } | { name: string }) => void;
```
