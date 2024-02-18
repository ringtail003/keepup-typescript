# Easier Implicit Returns for  undefined -Returning Functions

## TL;DR

明示的なreturnを書かない場合に `undefined` と推論されるようになった。

## Prev

戻り値の型に `void` や `any` を指定すると、return文なしが許容されていた。\
`undefined` は許容されなかった。

```typescript
// OK
function f1(): any {}
function f2(): void {}
function f3() {}

// ERROR: return文を書かないといけない
function f4(): undefined {
    // return;
    // return undefined;
}

// ERROR: return文を書かないといけない
declare function f5(f: () => undefined): undefined;
f5(() => {});
```

## Current

```typescript
// OK
function f4(): undefined {}

// OK
declare function f5(f: () => undefined): undefined;
f5(() => {});
```

`--noImplicitReturns` がオンの場合には明示的なreturnが必要。\
※TS Playground（5.1.6）ではreturnをコメントアウトしてもエラーにならなかった。

```typescript
function f6(): undefined {
    if (Math.random()) {
        return;
    }
}
```
