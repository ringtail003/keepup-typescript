# Support for \`export type \*\`

## TL;DR

`export type *` が使えるようになった。

```typescript
// OK
export type * from "a";

// OK
export type * as A from "a";
```
