# Resolution Customization Flags

## TL;DR

`tsconfig.json` にファイル解決のためのオプションが追加された。



`tsconfig.json` の設定項目を説明したページ。

[https://www.typescriptlang.org/ja/tsconfig](https://www.typescriptlang.org/ja/tsconfig#resolvePackageJsonImports)

## allowImportingTsExtensions

`.ts` や `.mts` 内の相互インポートができるようになる。\
`--noEmit` もしくは `--emitDecorationOnly` をオンにするとトランスパイル後のJSでインポートが解決できなくなる。これを解消するためのオプションらしい。

## resolvePackageJsonExports

`node_modules` 配下のパッケージを読み取る時に `package.json` の `exports` フィールドを参照する。 `--moduleResolution` で `node16 / nodenext` を指定するとデフォルトtrueになる。

## resolvePackageJsonImports

`package.json` の `import` のマッピングを参照する。

```typescript
// package.json
"import": {
    "#app": {
        "node": "app-node.js",
        "default": "app.js",
    }
}
```

```typescript
import app from "#app";
```

環境が `node` なら `app-node.js` 、条件指定がなければ `app.js` を参照する。

&#x20;`--moduleResolution` で `node16 / nodenext` を指定するとデフォルトtrueになる。
