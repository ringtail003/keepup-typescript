# Searching Ancestor Configuration Files for Project Ownership

## TL;DR

エディタを通してTSServeを利用している時の挙動が改善された。\
`.tsconfig.json` が最初にヒットしてもディレクトリツリーを辿って検索されるようになった。

```typescript
project/
├── src/
│   ├── foo.ts
│   ├── foo-test.ts
│   ├── tsconfig.json
│   └── tsconfig.test.json
└── tsconfig.json
```

上記のような場合に `.tsconfig.json` がヒットすると検索が止まっていたらしい。



