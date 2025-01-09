# Notable Behavioral Changes

## lib.d.ts

特に大きい影響がある変更はなさそう。

## `.tsbuildinfo` is Always Written <a href="#tsbuildinfo-is-always-written" id="tsbuildinfo-is-always-written"></a>

`--build` オプションでビルドした場合 `.tsbuildinfo` が必ず出力されるようになった。\
依存元プロジェクトにコンパイルエラーがあっても依存先プロジェクトのビルドを続行するようになった影響。

## Respecting File Extensions and `package.json` from within `node_modules` <a href="#respecting-file-extensions-and-package.json-from-within-node_modules" id="respecting-file-extensions-and-package.json-from-within-node_modules"></a>

`.mts` ファイルは `.mjs` を、 `.cts` は `.cjs` を出力するようになった。\
以前のバージョンでは `.mts` から `.cts` を読み込むとCommonJSの挙動に合わせる仕様になっていた？\
[https://zenn.dev/uhyo/articles/typescript-module-option](https://zenn.dev/uhyo/articles/typescript-module-option)\
\
リリースノートでもエコシステムでesmが増えてきたため、とある。

## Correct override Checks on Computed Properties

Symbolを使ったクラスのメンバのチェックが改善された。

```typescript
const foo = Symbol("foo");
const bar = Symbol("bar");

class Parent {
    [foo]() {}
}

class Child {
    override [bar]() {} // ERROR: ベースクラスに存在しないメンバのoverride
    [foo]() {} // ERROR: overrideキーワードがない
}
```
