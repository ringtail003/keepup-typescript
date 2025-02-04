# plan: Checked Return Statements for Conditional and Indexed Access Types

## TL;DR

引数によって分岐する戻り値の型推論が改善された。

```typescript
type Key = "A" | "B";

type Result = {
  "A": string;
  "B": string[];
}

function pick<K extends Key>(key: K): Result[K] {
  const values = ["foo", "bar"];

  if (key === "A") {
    return values[0]; // < v5.8 ERROR
  } else {
    return values; // < v5.8 ERROR
  }
}
```
