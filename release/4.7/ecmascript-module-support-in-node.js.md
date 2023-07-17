# ECMAScript Module Support in Node.js

## TL;DR



{% hint style="info" %}
Node.jsのESMとTSサポートがなかなかに複雑なので以下にメモした。

[https://ringtail003-til.firebaseapp.com/posts/2023-04-19-07-51](https://ringtail003-til.firebaseapp.com/posts/2023-04-19-07-51)
{% endhint %}

TS4.5の時点ではNode.js ESMはNightly Buildでの実験的機能だったが、4.7で晴れてStableになった。\
tsconfig.jsonのmodule指定が `node12` から `node16` に変わる。

## node16

{% code title="tsconfig.json" %}
```typescript
"module": "node16" 
```
{% endcode %}

* Top Level Awaitが使用できる。
* import / exportがそのままJSに出力される。
* requireやmoduleは使用できない。
* exportsで `.mjs/.d.mts` `.cjs/.d.cts` が出し分けできる。

## commonjs

{% code title="tsconfig.json" %}
```typescript
"module": "commonjs"
```
{% endcode %}

* require / module.exports = に変換される。

## package.jsonのexports

出力ファイルがESM/CommonJSどちらからも読み込めるよう、詳細な指定を記述できるようになった。\
従来の `types` `main` よりも優先される。

{% code title="package.json" %}
```typescript
exports: {
  ".": {
    require: {
      types: "./lib/index.d.cts",
      default: "./lib/index.cjs",
    },
    import: {
      types: "./lib/index.d.mts",
      default: "./lib/index.mjs",
    }
  }
},
types: "./types/index.d.ts", // Fall-back for older versions
main: "./lib/index.cjs", // Fall-back for older versions
```
{% endcode %}
