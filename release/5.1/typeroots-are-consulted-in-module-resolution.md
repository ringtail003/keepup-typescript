# typeRoots  Are Consulted In Module Resolution

## TL;DR

`tsconfig.json` の `typeRoots` の検索の挙動が変わった。

```typescript
compilerOptions: {
    types: [
        "node",
        "mocha",
    ],
    typeRoots: [
        "./vendor/types", // (1) @typesフォルダに含まれない型
        "./node_modules/@types", // (2) プロジェクト内のnpmパッケージの型
        "../../node_modules/@types", // (3) モノレポパッケージの型
    ]
}
```

以前のバージョンでは親フォルダを遡る挙動だったため(3)は記述がなくても(2)によって検索されていた。5.1から(3)は明示的な指定が必要。

## 参考

`tsconfig.json` には `types` というフィールドがある。指定した型はグローバルに含まれる。

```typescript
compilerOptions: {
    types: ["node", "jest", "express"]
}
```
