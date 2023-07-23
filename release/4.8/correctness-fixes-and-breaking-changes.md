# Correctness Fixes and Breaking Changes

## TL;DR <a href="#libdts-updates" id="libdts-updates"></a>

破壊的変更を含む改善が施された。

## `lib.d.ts`Updates <a href="#libdts-updates" id="libdts-updates"></a>

エラーのcauseはErrorオブジェクトでなくunknownになった。

## Unconstrained Generics No Longer Assignable to`{}` <a href="#unconstrained-generics-no-longer-assignable-to" id="unconstrained-generics-no-longer-assignable-to"></a>

以下の機能が破壊的変更を含む。\
暗黙のうちに許容していたnull,undefinedに対してエラーが発生する。

{% content-ref url="improved-inference-from-binding-patterns.md" %}
[improved-inference-from-binding-patterns.md](improved-inference-from-binding-patterns.md)
{% endcontent-ref %}

## Decorators are placed on modifiers on TypeScript’s Syntax Trees

ECMAScriptのデコレータの策定により、先行して実装していたTSとの齟齬が生じた。\
ECMAScriptのデコレータにアクセスするための関数が公開された。

```typescript
function canHaveModifiers(node: Node): node is HasModifiers;
function getModifiers(node: HasModifiers): readonly Modifier[] | undefined;
```

## Types Cannot Be Imported/Exported in JavaScript Files

JSファイルで型をエクスポートした場合に、ランタイムのESMでエラーが発生するようになった？（以前からなのか不明）。

JSで型をインポート・エクスポートするにはJSDocの `@typedef` で補完する。

## Binding Patterns Do Not Directly Contribute to Inference Candidates <a href="#binding-patterns-do-not-directly-contribute-to-inference-candidates" id="binding-patterns-do-not-directly-contribute-to-inference-candidates"></a>

以下の機能が破壊的変更を含む。\
推論結果が変わる影響によりエラーが発生する。

{% content-ref url="improved-inference-from-binding-patterns.md" %}
[improved-inference-from-binding-patterns.md](improved-inference-from-binding-patterns.md)
{% endcontent-ref %}

