# Optimized Checks for Ongoing Type Compatibility

## TL;DR

再帰型の比較が最適化されパフォーマンスが向上した。

```typescript
interface A {
    value: A;
    other: string;
}

interface B {
    value: B;
    other: number;
}
```

A, Bの互換性をチェックする時、型スタックに保存していたらしい。\
再帰が深いとパフォーマンスに問題があったが、v5.2で最適化された。
