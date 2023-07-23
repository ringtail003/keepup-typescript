# Correctness Fixes and Breaking Changes

## TL;DR <a href="#libdts-updates" id="libdts-updates"></a>

破壊的変更を含む改善が施された。

## `lib.d.ts`Updates <a href="#libdts-updates" id="libdts-updates"></a>

エラーのcauseはErrorオブジェクトでなくunknownになった。

## Unconstrained Generics No Longer Assignable to`{}` <a href="#unconstrained-generics-no-longer-assignable-to" id="unconstrained-generics-no-longer-assignable-to"></a>

以下の機能が破壊的変更を含む。\
暗黙のうちに許容していたnull,undefinedに対してエラーが発生する。

{% embed url="https://app.gitbook.com/s/9aquqlNjxpewEborrRfQ/~/changes/117/release/4.8/improved-intersection-reduction-union-compatibility-and-narrowing" %}

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

{% embed url="https://app.gitbook.com/s/9aquqlNjxpewEborrRfQ/~/changes/118/release/4.8/improved-inference-from-binding-patterns" %}

## Unused Renames in Binding Patterns are Now Errors in Type Signatures <a href="#unused-renames-in-binding-patterns-are-now-errors-in-type-signatures" id="unused-renames-in-binding-patterns-are-now-errors-in-type-signatures"></a>

関数の引数において名前なしのバインディングバターンが許容されなくなった。\
以下のような構文はエラーが発生する。

```typescript
// ❌ ERROR
declare function fn({ a: string, b: number }): void;

// 正しくはこのようになる
declare function fn({ a,b }:{ a: string, b: number }): void;

// もしくはオブジェクト自体に名前を付ける
declare function fn(args: { a: string, b: number }): void;
```

