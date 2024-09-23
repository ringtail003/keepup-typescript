# Control Flow Narrowing for Constant Indexed Accesses

## TL;DR

`obj[key]` がNarrowingされるようになった。

## example

```typescript
function f1(obj: Record<string, unknown>, key: string) {
  if (typeof obj[key] === "string") {
    obj[key].toUpperCase(); // Narrowing
  }

  if (typeof obj[key] === "number") {
    obj[key].toString(); // Narrowing
  }
}
```
