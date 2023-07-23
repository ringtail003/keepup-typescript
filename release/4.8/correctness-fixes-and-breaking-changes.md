# Correctness Fixes and Breaking Changes

## TL;DR <a href="#libdts-updates" id="libdts-updates"></a>

破壊的変更を含む改善が施された。

## `lib.d.ts`Updates <a href="#libdts-updates" id="libdts-updates"></a>

エラーのcauseはErrorオブジェクトでなくunknownになった。

## Unconstrained Generics No Longer Assignable to`{}` <a href="#unconstrained-generics-no-longer-assignable-to" id="unconstrained-generics-no-longer-assignable-to"></a>

4.8で導入された以下の改善が破壊的変更を含む。\
暗黙のうちに許容していたnull,undefinedに対してエラーが発生する。

{% content-ref url="improved-inference-from-binding-patterns.md" %}
[improved-inference-from-binding-patterns.md](improved-inference-from-binding-patterns.md)
{% endcontent-ref %}

## Decorators are placed on modifiers on TypeScript’s Syntax Trees

ECMAScriptのデコレータの策定により、先行して実装していたTypeScriptの齟齬が生じた。\
ECMAScriptのデコレータにアクセスするための関数が公開された。

```typescript
function canHaveModifiers(node: Node): node is HasModifiers;
function getModifiers(node: HasModifiers): readonly Modifier[] | undefined;
```
