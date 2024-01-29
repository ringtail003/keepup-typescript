# Unlisted Property Narrowing with the in Operator

## TL;DR

in演算子に続いたプロパティアクセスのエラーが解消した。

```typescript
declare const json: unknown;

if (json && typeof json === "object") {
    if (
        "name" in json && 
        typeof json.name === "string" // ~v4.8: ERROR
    ) {
        console.log(json.name); // ~v4.8: ERROR
    }
}
```

v4.9から `A in obj` で評価された後 `obj.A` の存在が担保されるようになった。

* obj は `unknown & record<"A", unknown>` 型と推論される
* `A in obj` の時、Aが `string | number | Symbol` であることの型チェックが入る

