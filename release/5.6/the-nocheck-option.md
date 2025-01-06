# The --noCheck Option

## TL;DR

型チェックをスキップするオプションが追加された。

```typescript
// tsconfig.json
noCheck: true
```



オプションをオンにすると以下のメリットがある。

* `tsc --noCheck` と `tsc --noEmit` でJSの生成と型チェックのフェーズを分離できる。
* `--isolatedDeclarations` と併用して型チェックなしで型宣言ファイルをすばやく生成できる。
* モジュールや型のトランスパイルのみ実行したい場合、型チェックをスキップすると高速化できる。
