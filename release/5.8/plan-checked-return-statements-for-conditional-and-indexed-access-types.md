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

以前のバージョンでは `cond ? A : B`のように分岐する場合、戻り値の型はunion（ `A | B` ）と推論されていた。上記例の場合、unionにanyが含まれるためstringを許容している。

v5.8から `A` と `B` それぞれの型が戻り値とマッチするかチェックされるようになった。
