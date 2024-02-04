# Correctness Fixes and Breaking Changes

## \`lib.d.ts\` 更新

破壊的変更はないらしい。

## Better Types for \`Promise.resolve\`

Promiseは常に `Awaited` を返していたが `any` や `unknown` を期待することがあり、それらも返すようになった。

## JavaScript Emit No Longer Elides Imports

JSからTSにコンパイルした時、値のインポートは残るが、型のインポートは削除されていた。型インポートも残るようになった。

## exports is Prioritized Over typesVersions

`package.json` の `typesVersions` 相当のものが `exports` で指定できるようになった。

```typescript
{
    "typesVersions": {
        "<4.8": { ".": [ "4.8-types/main.d.ts" ] }, // old
        "*": { ".": [ "modern-types/main.d.ts" ] }, // old
    },
    "exports": {
        "types@<4.8": "4.8-types/main.d.ts", // new
        "types": "modern-types/main.d.ts", // new
    }
}
```
