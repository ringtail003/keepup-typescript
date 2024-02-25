# Breaking Changes

## ES2020 and Node.js 14.17 as Minimum Runtime Requirements

TSがES2020の機能を使用するため、ビルドの必須環境がNode v14.17になった。

## Explicit typeRoots Disables Upward Walks for node\_modules/@types

以前のバージョンでは `typeRoots` のディレクトリ解決に失敗した時、親フォルダを遡って `node_modules/@types` を解決しようとしていた。\
5.1でこの挙動がなくなった。
