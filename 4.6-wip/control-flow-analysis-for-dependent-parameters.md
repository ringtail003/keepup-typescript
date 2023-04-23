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

## 解いてみる

愚直に実装したケース：

```typescript
const fn1 = (kind: string, query: string | number) => {
  // 🤔 うっかりタイポしても気づけない
  if (kind === 'idddd') {}

  // 🤔 型の不一致
  if (kind === 'name') {
    search({ name: query }); // ❌ ERROR! string|number型をstringに渡そうとしている
  }
}
```

タグ付きユニオンで実装したケース：

```typescript
const fn2 = (obj: { kind: 'id', query: number } | { kind: 'name', query: string }) => {
  // 👍 タイポや型の不一致でコンパイルエラーが得られ、型安全になる
  if (obj.kind === 'idddd') { // ❌ ERROR! 
    search({ id: obj.query });
  }

  if (obj.kind === 'name') {
    search({ name: obj.query });
  }
}

// 🤔 引数をオブジェクトで受け取らないといけない、場合によっては冗長
```

タプルで実装したケース：

```typescript
type F3 = (...args: ['id', number] | ['name', string]) => void;

// 👍 引数がオブジェクトに縛られない
const fn3: F3 = (kind, query) => {
  if (kind === 'id') {
    search({ id: query });
  }

  if (kind === 'name') {
    search({ name: query });
  }
};

fn3('id', 123);
fn3('name', '山田');
```

自分で実装する分にはタグ付きユニオンを使うケースが多いかもしれない。\
サードパーティのライブラリとの中継型としてユースケースが考えられる。

```typescript
import { fn } from 'vendor';

// wip
fn('id', 123);
```
