# Optimizations by Comparing Non-Normalized Intersections

## TL;DR

交差型の比較が最適化された。

```typescript
A & (B | C)
```

交差型は以下のように変換される。

```typescript
(A & B) | (A & C)
```

共有型の数が多い場合にはパフォーマンスが問題となる。

```typescript
A & (B1 | B2 | B3 | B4 ... B9999)

(A & B1)｜（A & B2
```
