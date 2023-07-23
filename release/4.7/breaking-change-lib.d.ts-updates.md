# BREAKING CHANGE: lib.d.ts Updates

## TL;DR

lib.d.tsに対するいくつかの破壊的変更が入った。

## Stricter Spread Checks in JSX

JSXでスプレッド構文の対象がオブジェクトであることがチェックされるようになった。

```typescript
function MyComponent(props: unknown) {
    
    // ❌ ERROR
    return <div {...props} />;
}
```

## Stricter Checks with Template String Expressions

Symbolのテンプレート構文への展開が厳密にチェックされるようになった。

```typescript
// ❌ ERROR
function fn<T extends symbol>(key: T): string {
    return `${key}`;
}

// ❌ ERROR
let str = `${Symbol()}`;
```

Stringでラップすることで回避できる。

```typescript
function fn<T extends symbol>(key: T): string {
    return `${String(key)}`;
}

let str = `${String(Symbol())}`;
```

## readFile Method is No Longer Optional on LanguageServiceHost

Reactのモジュール読み込みに起因した修正のようなのでスキップ。

## readonly Tuples Have a readonly length Property

読み取り専用のタプルのlengthを変更できなくなった（バグ修正のよう）。

```typescript
type Tuple = readonly [boolean, boolean];
declare const tuple: Tuple;

tuple.length = 1; // ❌ ERROR
```
