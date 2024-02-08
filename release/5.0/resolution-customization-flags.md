# Resolution Customization Flags

## TL;DR

`tsconfig.json` にファイル解決のためのオプションが追加された。

## allowImportingTsExtensions

`.ts` や `.mts` 内の相互インポートができるようになる。\
`--noEmit` もしくは `--emitDecorationOnly` をオンにするとトランスパイル後のJSでインポートが解決できなくなる。これを解消するためのオプションらしい。

## resolvePackageJsonExports

