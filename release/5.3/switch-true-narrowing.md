# \`switch (true)\` Narrowing

## TL;DR

`switch(true)` と抱合せで `case` に条件判定を指定できるようになった。

## use-cases

```typescript
function f(x: unknown) {
    switch (true) {
        case typeof x === "string": ...
        case Array.isArray(x): ...
        default:
    }
}
```
