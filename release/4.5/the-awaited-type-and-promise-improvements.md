# The Awaited Type and Promise Improvements

## TL;DR

Promiseをアンラップするビルトインユーティリティタイプが追加された。

## Promiseの雑な説明

JavaScriptの `fetch` を例に説明する。この関数はAPIリクエストを実行する。

```javascript
fetch("https://my-domain/users/102");
```

リクエストが完了するのは、1秒後かもしれないし10秒後かもしれない。このような非同期処理を扱うため `fetch()` の戻り値は `Promise` というオブジェクトになっている。

```javascript
fetch("https://...");

// Promise {
//   then: () => {}
//   catch: () => {}
// }

fetch("https://...")
  // リクエストが正常終了した時の処理
  .then(() => {
    console.log("レロレロ レロレロ レロレロレロ");
  })
  // リクエストが失敗した時の処理
  .catch(() => {
    console.log("『てめーはおれを怒らせた』");
  })
;
```

TypeScriptは `Promise` オブジェクトの型情報を持っている。

```typescript
async function fetchUser() {
  return fetch("https://...");
}

// 戻り値がPromiseだと推論される
// Promiseはthenメソッドを持っている
// thenには関数を渡すことができる
fetchUser().then(() => { ... });
```

型情報のおかげで、タイポも見つけやすくなるし、値を推論することもできる。

```typescript
fetchUser().theeeeen(); // Error
```

```typescript
// 推論によって「id」がエディタでサジェストされる
fetchUser().then(user => {
  console.log(user.id);
});
```

では、`fetchUser` の返す値を「型」として定義したい時はどうするか。\
愚直にやるならレスポンスをインターフェースにすれば良い。

```typescript
async function fetchUser() {
  return { id: 1 };
}
type FetchUserType = Promise<{ id: number }>;
```

ただしこの方法は、エンドポイントごとに同じような型を定義するわずらわしさがある。

```typescript
type FetchUserType = Promise<{ id: number }>;
type FetchUserProfileType = Promise<{ id: number; age: number; }>;
type FetchUserHistoryType = Promise<{ id: number; lastLoggedIn: date; }>;
...
```

さらにリクエストを扱う関数など、型の扱いがやっかいになる。

{% code overflow="wrap" %}
```typescript
// 全部型を列挙するか？
async function fn(fetch: () =>
  FetchUserType | FetchUserProfileType | FetchUserHistoryType
) {
  return {
    data: await fetch(),
    requestedAt: new Date(),
  };
}

// anyで許容するか？
async function fn2(fetch: () => Promise<any>) {
  return {
    data: await fetch(),
    requestedAt: new Date(),
  };
}
```
{% endcode %}

## Promiseから値の型を導出（従来）

`Promise` の値を取り出すときは `infer` で推論するのが通例だった。

```typescript
async function fetchUser() {
  return { id: 1 };
}

// (1)fetchUser関数の戻り値を型定義する
type FetchUserReturnType = ReturnType<typeof fetchUser>;

// (2)Promise<R>からRを取り出す
type UnWrapped<T> = T extends Promise<infer R> ? R : T;

// (3)Promise<fetchUserの戻り値>から戻り値を取り出す
type XXX = UnWrapped<FetchUserReturnType>;
// { id: number }
```

## Pormiseから値の型を導出（新）

**Awaited** というビルトインの型が追加された。このビルトイン型が今回追加された機能。\
`infer` を使った導出は `Awaited` に置き換えることができる。

```typescript
async function fetchUser() {
  return { id: 1 };
}

// (1)fetchUser関数の戻り値を型定義する
type FetchUserReturnType = ReturnType<typeof fetchUser>;

// (2)Promise<fetchUserの戻り値>から戻り値を取り出す
type XXX = Awaited<FetchUserReturnType>;
// { id: number }
```

## Awaitedの優れた点：ネストに対応

Promiseのネストを表現してみる。

```typescript
function promisefy<T>(value: T): Promise<T> {
    return new Promise(resolve => resolve(value));
}

const promise = promisefy(promisefy(promisefy(100)));
// promise:Promise<Promise<Promise<number>>>
```

Promiseの内包するnumberを取り出すにはどうしたらいいだろうか？\
前述の `infer` を使った導出はネストに対応していない。

{% code title="inferを使った導出" %}
```typescript
type UnWrapped<T> = T extends Promise<infer R> ? R : T;

type XXX = UnWrapped<typeof promise>;
// Promise<Promise<number>
// まだネストしてる

type YYY = UnWrapped<UnWrapped<UnWrapped<typeof promise>>>;
// number
// ネストの数に依存している
```
{% endcode %}

がんばれば値を導出できるが「開発者が都度がんばる」というところに問題がある。

{% code title="開発者ががんばる" overflow="wrap" %}
```typescript
type UnWrapped<T> = T extends Promise<infer R> 
  ? R extends Promise<any> ? UnWrapped<R> : R 
  : T;

type XXX = UnWrapped<typeof promise>;
// number
// 導出できた....けど...UnWrappedの型、読める？？
```
{% endcode %}

`Awaited` を使うと簡単に値を導出できる。

```typescript
type XXX = Awaited<typeof promise>;
// number
```
