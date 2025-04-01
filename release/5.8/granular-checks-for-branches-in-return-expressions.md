# Granular Checks for Branches in Return Expressions

## TL;DR

引数によって分岐する戻り値の型推論が改善された。

```typescript
declare const untypedCache: Map<any, any>;

function getUrlObject(urlString: string): URL {
    return untypedCache.has(urlString) ?
        untypedCache.get(urlString) :
        urlString;
    //  ~~~~~~~~~
    // error! Type 'string' is not assignable to type 'URL'.
}
```

以前のバージョンでは `cond ? trueBranch : falseBranch`のように分岐する場合、各ブランチの型チェック
