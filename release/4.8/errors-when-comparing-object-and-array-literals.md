# Errors When Comparing Object and Array Literals

## TL;DR

オブジェクト・配列において、リテラルとの比較が禁止されるようになった。\
必ずfalseを返す意味のない比較のため。

```typescript
const obj = {};
if (obj == {}) {} // ❌ ERROR
if (obj === {}) {} // ❌ ERROR

const array: string[] = [];
if (array == []) {} // ❌ ERROR
if (array === []) {} // ❌ ERROR
```
