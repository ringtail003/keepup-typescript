# Object.groupBy and Map.groupBy

## TL;DR

`ES2024` の `Object.groupBy` と `Map.groupBy` の型が実装された。

tsconfig.jsonの `target` を `ES2024` または `ESNext にセットする必要がある。`

## Example

```typescript
const myObj = Object.groupBy([0,1,2,3,4], (num, index) => {
    return num % 2 === 0 ? "even": "odd";
});

// myObj:
// {
//   even?: number;
//   odd?: number;
// };
```

```typescript
const myMap = Map.groupBy([0,1,2,3,4], (num, index) => {
    return num % 2 === 0 ? "even" : "odd";
});

// myMap: Map<"even" | "odd", number[]>
```

ObjectまたはMapのキー `even` `odd` はオプショナル。\
ソースとして与えられた配列に該当する値が存在しなければ、これらのキーは生成されないため。
