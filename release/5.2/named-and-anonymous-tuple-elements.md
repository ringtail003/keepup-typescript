# Named and Anonymous Tuple Elements

## TL;DR

名前付きTupleで使えるシンタックスが増えた。

```typescript
type Pair<T> = [first:T, T];

type T2<T> = [first:T, ...T[]];
```
