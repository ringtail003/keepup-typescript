# typeRoots  Are Consulted In Module Resolution

## TL;DR

`tsconfig.json` の `typeRoots` で親フォルダを遡る時の挙動が変わった？

```typescript
compilerOptions: {
    typeRoots: [
        "./vendor/types", // (1) @typesフォルダに含まれない型
        "./node_modules/@types", // (2) プロジェクト内のnpmパッケージの型
        "../../node_modules/@types", // (3) モノレポパッケージの型
    ]
}
```

今まで(3)は記述がなくても検索されていたが、過剰な検索を避けるため記述が必要になった。という事だと思う。

## 参考

`tsconfig.json` には `types` というフィールドがある。指定した型はグローバルに含まれる。

```typescript
compilerOptions: {
    types: ["node", "jest", "express"]
}
```
