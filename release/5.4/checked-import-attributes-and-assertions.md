# Checked Import Attributes and Assertions

## TL;DR

`import X from Y with { type: Z }` のタイプがチェックされるようになった。

```typescript
import * as ns from "foo" with { type: "not-json" }; // ERROR
```

&#x20;`type` の文字列は `ImportAttributes` で宣言される。

```typescript
interface ImportAttributes {
  type "json";
}
```
