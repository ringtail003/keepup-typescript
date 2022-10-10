# The Awaited Type and Promise Improvements

Promiseをアンラップするビルトインユーティリティタイプが追加された。

## Promiseの雑な説明

JavaScriptの `fetch` を例に説明する。この関数はAPIリクエストを実行する。

```javascript
// リクエストが完了するのは、1秒後かもしれないし10秒後かもしれない。
fetch("https://my-domain/users/102");
```

`fetch()` の戻り値は `Promise` というオブジェクトである。

```javascript
fetch("https://...");

// Promise {
//   then: () => {}
//   catch: () => {}
// }
```

`Promise` オブジェクトのメソッドを使うと、コールバックを実行できる。

```javascript
fetch("https://...").then((user) => {
  console.log(user.id);
});
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
async function fetchUser() {
    return { id: 1 };
}

fetchUser().then(user => {
  console.log(user.id);
});
```

では、`fetchUser` の返す「値」を「型」として定義したい時はどうするか？これが追加された機能の本題になる。

<pre class="language-typescript"><code class="lang-typescript">async function fetchUser() {
    return { id: 1 };
}

<strong>type FetchUserType = Promise&#x3C;{ id: 1 }>;
</strong><strong>
</strong><strong>// { id:1 } をうまいこと型定義できないか？</strong></code></pre>

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
type FetchUserType = UnWrapped<FetchUserReturnType>;

// FetchUserType: { id: number }
```

## Pormiseから値の型を導出（新）

**Awaited** というビルトインの型が追加された。`infer` を使った導出は `Awaited` に置き換えることができる。

```typescript
async function fetchUser() {
    return { id: 1 };
}

// (1)fetchUser関数の戻り値を型定義する
type FetchUserReturnType = ReturnType<typeof fetchUser>;

// (2)Promise<fetchUserの戻り値>から戻り値を取り出す
type FetchUserType = Awaited<FetchUserReturnType>;

// FetchUserType: { id: number }
```

## Awaitedの優れた点：ネストに対応

Promiseのネストを表現してみる。もうちょっとマシなサンプルを書きたかったけど初見殺しなコードしか書けなかった...。

```typescript
function promisefy<T>(value: T): Promise<T> {
    return new Promise(resolve => resolve(value));
}

const promise = promisefy(promisefy(promisefy(100)));

// ３重にネストしたPromiseになる。
// promise:Promise<Promise<Promise<number>>>

// お題：ここから「number」を取り出すには？
```

前述の `infer` を使った導出はネストに対応していない。がんばれば値を導出できるが「開発者が都度がんばる」というところに問題がある。

{% code title="前述の導出を使う" %}
```typescript
type UnWrapped<T> = T extends Promise<infer R> ? R : T;

type XXX = UnWrapped<typeof promise>;
// Promise<Promise<number>
// まだネストしてる
```
{% endcode %}

{% code title="開発者ががんばる" overflow="wrap" %}
```typescript
type UnWrapped<T> = T extends Promise<infer R> ? R extends Promise<any> ? UnWrapped<R> : R : T;

type XXX = UnWrapped<typeof promise>;
// number
// 導出できた....けど...UnWrappedの型、読める？？
```
{% endcode %}

:tada: `Awaited` を使うと、いとも簡単に値を導出できる。

```typescript
type XXX = Awaited<typeof promise>;
// number
```
