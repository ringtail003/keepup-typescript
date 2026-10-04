# Less Context-Sensitivity on  this -less Functions

### Previous

```typescript
declare function callIt<T>(obj: {
    produce: (x: number) => T,
    consume: (y: T) => void,
}): void;

callIt({
    produce(x: number) { return x * 2; },
    consume(y) { return y.toFixed(); },
});

callIt({
    consume(y) { return y.toFixed(); },
    //                  ~
    // error: 'y' is of type 'unknown'.
    produce(x: number) { return x * 2; },
});
```

consumeは引数に型がなく、このような関数はContextually sensitive functionsと呼ばれる。

T型はproduceの実体から推論される。\
consumeを評価するタイミングでproduceがまだ評価されていないため、Tが推論できない。

この書き方はアロー関数では発生しない。\
functionが暗黙のthisを持つため、consume評価でthisを参照し型推論に失敗している。

### Current

文脈依存の関数について評価を遅延し型解決してから評価されるよう考慮が加えられた。\
function構文でもエラーが発生しなくなった。<br>

