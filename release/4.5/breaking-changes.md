# Breaking Changes

{% embed url="https://media.giphy.com/media/TfY3cjjH0aYopkybqc/giphy.gif" %}

## lib.d.ts Changes

Promiseのメソッドの戻り値の型がAwaitedに変わった。

[https://github.com/microsoft/TypeScript/pull/45350](https://github.com/microsoft/TypeScript/pull/45350)

* Promise.all
* Promise.race
* Promise.allSettled
* Promise.any

{% tabs %}
{% tab title="従来" %}
```typescript
function foo<T>(p: Promise<T>) {
  // Promise<T[]>
  return Promise.all<T>([p]);
}
```
{% endtab %}

{% tab title="V4.5" %}
```typescript
function foo<T>(p: Promise<T>) {
  // Promise<Awaited<T>[]>
  return Promise.all<T>([p]);
}
```
{% endtab %}
{% endtabs %}

#### 影響

`Promise.all` の戻り値を `Promise<T[]>` 型の変数に代入しているような場合、書き換える必要がある。\


## Compiler Options Checking at the Root of tsconfig.json

tsconfig.jsonの「compilerOption」で宣言するべきオプションが「compilerOption」の外にある時、警告されるようになった。

{% tabs %}
{% tab title="従来" %}
```typescript
// tsconfig.json
{
  "target": "2017", // <=== 本来「compilerOptions」の下にないといけないやつ
  "compilerOptions": {
    ...
  }
}
```

{% hint style="success" %}
エラーなし
{% endhint %}
{% endtab %}

{% tab title="V4.5" %}
```typescript
// tsconfig.json
{
  "target": "2017", // <=== 本来「compilerOptions」の下にないといけないやつ
  "compilerOptions": {
    ...
  }
}
```

{% hint style="danger" %}
{% code overflow="wrap" %}
```
Error: 'target' should be set inside the 'compilerOptions'object of the config json file
```
{% endcode %}
{% endhint %}
{% endtab %}
{% endtabs %}

#### 影響

スルーされていた記述ミスがエラーとして検出された場合、tsconfig.jsonを書き換える必要がある。

##
