# Control Flow Analysis for Dependent Parameters

## TL;DR

関数の引数で、タグ付きユニオンと同じことがタプルでもできるようになった。

## 前提

タグ付きユニオンとは、オブジェクトのプロパティから他のプロパティの型を推論する仕組み。

```typescript
const fn = (obj: 
  { kind: 'n', payload: number } | { kind: 's', payload: string }
) => {
  
  if (obj.kind === 'n') {
    obj.payload.toFixed();
  }
  
  if (obj.kind === 's') {
    obj.payload.concat('b');
  }
}
```

タプルとは、特定の順番で並んだ型。

```typescript
type Tuple = [string, number, boolean];

const t1: Tuple = ['a', 1, true];
const t2: Tuple = ['b', 0, false];
```

## お題

ユーザーの入力を受け取ってsearch関数にわたすロジックを考える。\
引数は `id（数値）`または `name（文字列）` のどちらかとする。

```typescript
declare const search: (params: { id: number } | { name: string }) => void;

search({ id: 1 });
search({ name: 'foo' });
```

### queryを分岐させるパターン

```typescript
const fn1 = (kind: string, query: string | number) => {
  if (kind === 'id') {
    search({ id: query }); // ❌ ERROR! 
  }
  if (kind === 'name') {
    search({ name: query }); // ❌ ERROR! 
  }
}
```

queryが `string | number` なので `number` の引数に渡すと型の不一致が起こる。\
仕方なく `as number` など使うことになる。

### タグ付きユニオンで分岐させるパターン

```typescript
const fn2 = (obj: 
  { kind: 'id', query: number } | { kind: 'name', query: string }
) => {
  if (obj.kind === 'id') {
    search({ id: obj.query });
  }
  if (obj.kind === 'name') {
    search({ name: obj.query });
  }
}
```

実装都合で引数をオブジェクトに変える必要がある。

### タプルで分岐させるパターン

```typescript
type F3 = (...args: ['id', number] | ['name', string]) => void;

const fn3: F3 = (kind, query) => {
  if (kind === 'id') {
    search({ id: query });
  }
  if (kind === 'name') {
    search({ name: query });
  }
};
```

自分で実装する分にはタグ付きユニオンを使うケースが多いかもしれない。\
サードパーティのライブラリとの中継型としてユースケースが考えられる。

```typescript
import { fn } from 'vendor';

// wip
fn('id', 123);
```
