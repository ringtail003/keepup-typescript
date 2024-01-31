# Checks For Equality of \`NaN\`

## TL;DR

NaNの厳密等価は意味がないためエラーとして扱われるようになった。

```typescript
0 === NaN // ERROR: Did you mean 'Number.isNaN(...)'?
```

NaNかどうか調べる場合は `Number.isNaN` を使う。

