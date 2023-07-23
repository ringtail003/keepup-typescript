# --build --watch and --incremental Performance Improvements

## TL;DR

パフォーマンスが改善された。

## incrementalフラグ

ビルドキャッシュを有効にする。キャッシュは./lib/tsbuildinfoというファイルに吐き出される。\
ファイル名は任意に変更可能。

```typescript
{
  "compilerOptions": {
    "incremental": true,
    "tsBuildInfoFile": "./buildcache/front-end",
    ...
```
