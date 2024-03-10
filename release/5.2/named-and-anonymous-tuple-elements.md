# Named and Anonymous Tuple Elements

## TL;DR

名前付きTupleで使えるシンタックスが増えた。

```typescript
type Pair<T> = [first:T, T];

type T2<T> = [first:T, ...T[]];
```

Named Tupleによって型として取り出した後も名前が保持される。\
T3.latを取り出す方法は分からなかった。ここから先の利便性は不明。

```typescript
declare function fn(lat:number, lng: number): void;

type T3 = Parameters<typeof fn>;
// [lat: number, lng: number]
```
