# Isolated Declarations

## TL;DR

新しいオプション `isolatedDeclaration` が導入された。

目的は型定義ファイル `.d.ts`の生成を高速化すること。\
TypeScriptのコンパイルではimportなど依存関係を辿って関数の戻り値を推論する。\
これには多くの時間が費やされる。

`isolatedDeclaration` を指定すると型推論を高速化するための型アノテーションが強制される。

{% code overflow="wrap" %}
```typescript
// ERROR: Function must have an explicit return type annotation with --isolatedDeclarations.
export function f1() {
  ...
}

// FIX: 
export function f1(): number {}
```
{% endcode %}

推論が容易な箇所では、型アノテーションは強制されない。

```typescript
export function f2() {
  return 100;
}
```

`isolatedDeclaration` オプションは今後改善が加えられる予定。\
v5.5ではクラスなど未適用の部分がある。
