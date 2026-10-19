# Breaking Changes and Deprecations in TypeScript 6.0

## Overview

v7.0 の準備に向けて v6.0 ではいくつか破壊的変更が入っている。

* rootDir
  * tsconfig.json が配置されたディレクトリがルート扱いになった。src 配下が暗黙のルートになっていたプロジェクトでは src を指定しなおす必要がある。
* strict
  * true がデフォルト
* module
  * esnext がデフォルト、つまり ESM
  * `amd` `umd` `systemjs` `none` がサポートされなくなった
* types
  * `[ ]` がデフォルト、`node_modules/@types` を暗黙的に読み込まなくなった
* target
  * es5 が非推奨になった
* baseUrl
  * 非推奨になった
* moduleResolution
  * `classic` がサポートされなくなった
* outFile
  * 削除された

