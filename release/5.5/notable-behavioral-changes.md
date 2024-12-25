# Notable Behavioral Changes

#### Disabling Features Deprecated in TypeScript 5.0 <a href="#disabling-features-deprecated-in-typescript-50" id="disabling-features-deprecated-in-typescript-50"></a>

いくつかのオプションが非推奨になった。\
target: ES3\
noImplicitUseStrict\
など。

v5.0以降これらはignoreDeprecations:"5.0"を追加することで仕様できたが、v5.5ではオプション指定しても無効になる。tsconfig.jsonに存在できるがv6.0は存在自体がエラーとなる予定。

#### `lib.d.ts` Changes <a href="#libdts-changes" id="libdts-changes"></a>

公式ドキュメントには詳細な記載なし。

#### `undefined` is No Longer a Definable Type Name <a href="#undefined-is-no-longer-a-definable-type-name" id="undefined-is-no-longer-a-definable-type-name"></a>

TypeScriptではビルトインの型と同じ名前の型宣言はできない。

```typescript
type null = any;
type number = any;
type object = any;
```

undefinedのみ型宣言できていたが、v5.5から禁止された。

```typescript
type undefined = any; // v5.5+ ERROR
```
