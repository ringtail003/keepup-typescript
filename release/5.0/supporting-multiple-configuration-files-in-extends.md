# Supporting Multiple Configuration Files in \`extends\`

## TL;DR

`tsconfig.json` の `extends` に配列指定ができるようになった。

## サンプル

```typescript
{
    "extends": ["a.json", "b.json", "c.json"],
```

以下に等しい。

```typescript
// tsconfit.json
{ "extends": "c.json" }

// c.json
{ "extends": "b.json" }

// b.json
{ "extends": "a.json" }

// a.json
{ }
```
