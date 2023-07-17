# Instantiation Expressions

## TL;DR

アロー関数を使わずにジェネリクスを使った関数呼び出しが書けるようになった。

## Prev

```typescript
function f<T>(value: T) {
    return value;
}

const byString = (param:string) => f(param);
const byNumber = (param:number) => f(param);

byString("foo");
byNumber(1);
```

## Current

```typescript
function f<T>(value: T) {
    return value;
}

const byString = f<string>;
const byNumber = f<number>;

byString("foo");
byNumber(1);
```
