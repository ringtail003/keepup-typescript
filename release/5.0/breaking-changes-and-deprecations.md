# Breaking Changes and Deprecations

## Runtime Requirements

Node v10以降で動かすことが必須となった。

## \`lib.d.ts\` Changes

DOMインターフェースに変更が入った。

## Forbidden Implicit Coercions in Relational Operators

暗黙の変換を含む演算がエラーで検出されるようになった。

```typescript
function fn(ns: number | string) {
    ns * 4; // ERROR
    +ns * 4; // OK
}
```

## Enum Overhaul

Enumの範囲外の値がエラーで検出されるようになった。

```typescript
enum SomeEvenDigit {
    Zero = 0,
    Two = 2,
    Four = 4,
}

let m: SomeEvenDisit = 1; // ERROR
```

## More Accurate Type-Checking for Parameter Decorators in Constructors Under --experimentalDecorators

`experimentalDecorators` の型チェックが厳密になった。

## Deprecations and Default Changes <a href="#deprecations-and-default-changes" id="deprecations-and-default-changes"></a>

`tsconfig.json` の一部指定が `deprecated` とマークされた。

```typescript
// これらはv5.5で削除される
--target: ES3
--out
--noImplicitUseStrict
--keyofStringOnly
--suppressExcessPropertyErrors
--suppressImplicitAnyIndexErrors
--noStrictGenericChecks
--charset
--importsNotUsedAsValues
--preserveValueImports
--prepend in project reference
```

また一部指定が変更になった。

```typescript
--newLine
--forceConsistentCasingInFileNames
```
