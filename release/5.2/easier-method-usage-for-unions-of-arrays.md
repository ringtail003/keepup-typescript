# Easier Method Usage for Unions of Arrays

## TL;DR

Unionに対するfilterでエラーが出なくなった。

```typescript
declare let array: string[] | number[];
array.filter(x => !!x);
```

以前のバージョンは `x:any` と推論される。\
v5.2から `x:string | number` と推論されるようになった。
