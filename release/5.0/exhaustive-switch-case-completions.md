# Exhaustive switch/case Completions

## TL;DR

`switch` 文でcaseに列挙するリテラルがサジェストされるようになった。

```typescript
type Fruit = 
    | { kind: "apple" },
    | { kind: "banana" },
    | { kind: "orange" };
    
switch (fruit.kind) {
    case "apple":
    case "banana":
    case "orange":
}
```
