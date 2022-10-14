# Disabling Import Elision

コンパイラオプション `--preserveValueImports` が導入された。

## 未使用のインポート

インポートされた変数や型がTypeScriptによって「未使用」と判断される時、トランスパイル後のJavaScriptには出力されない。

この挙動で困るのはevalを使ってランタイムで変数を参照するようなケース。

```typescript
// foo.ts
export const foo = "FOO";
```

```typescript
// bar.ts
import { foo } from "./foo";
console.log(eval("foo")); // <=====
```

```shell
./node_modules/.bin/tsc ./bar.ts
```

```javascript
// bar.js（トランスパイルによって出力されたJavaScript）
console.log(eval("foo"));
export {};
```

`eval` は与えられたテキストをJavaScriptのコードとして実行する。`foo` の実体はトランスパイラによって削除されているためランタイムエラーが発生する。

```bash
ReferenceError: foo is not defined
```

セキュリティの問題もあり `eval` でコードを評価することはほとんどなくなったが、SvelteやVueで似たような問題が起こる。

```typescript
<!-- A .vue File -->
<script setup>
  import { someFunc } from "./some-module.js";
</script>
<button @click="someFunc">Click me!</button>
```

## --preserveValueImports

新しく導入されたオプションで未使用のインポートをコントロールできるようになった。

{% hint style="info" %}
このオプションは `module:es2015` 以降とセットで使用する。
{% endhint %}

### false（デフォルト）

以前のバージョンと同じ挙動。未使用のインポートが削除される。

```bash
./node_modules/.bin/tsc ./bar.ts \
  --module es2022 \
  --preserveValueImports false
```

```javascript
// bar.js（トランスパイルによって出力されたJavaScript）
console.log(eval("foo"));
export {};
```

### true

未使用のインポートが出力される。

```shell
./node_modules/.bin/tsc ./bar.ts \
  --module es2022 \
  --preserveValueImports true
```

```javascript
// bar.js（トランスパイルによって出力されたJavaScript）
import { foo } from "./foo";
console.log(eval("foo"));
```

## --isolatedModules

`preserveValueImports` を `isolatedModules` オプションと同時に指定する場合、型のインポートで注意が必要。

{% hint style="info" %}
`isolatedModules` はTypeScriptの標準コンパイラ `tsc` 以外でトランスパイルすることを目的としたオプション。esbuildのように複数の `.ts` をまとめて単一のファイル `.js` に出力するものがあり `isolatedModules` はそこに配慮して「単一ファイルに出力すると不整合が起こる」ことを警告する。
{% endhint %}

```typescript
// foo.ts
export type Foo = {};
```

```typescript
// bar.ts
import { Foo } from "./foo";
const foo: Foo = {};
```

上のコードを `isolatedModules:true` にして `tsc` に渡すとエラーが発生する。

```bash
./node_modules/.bin/tsc ./bar.ts \
  --module es2022 \
  --preserveValueImports true \
  --isolatedModules true

error TS1444: 
'Foo' is a type and must be imported usng a type-only import when 'preserveValueImports' and 'isolatedModules' are both enabled.
```

このエラーは「単一ファイルに出力すると型情報が消えてしまう」ことを警告している。

解決策は「型のみインポートする」シンタックスに書き換えること。

```typescript
// import { Foo } from "./foo";
import type { Foo } from "./foo";

const foo: Foo = {};
```

```bash
./node_modules/.bin/tsc ./bar.ts \
  --module es2022 \
  --preserveValueImports true \
  --isolatedModules true

# OK
```
