# Control-Flow Analysis for Bracketed Element Access

## TL;DR

ブラケット指定のプロパティで初期化忘れに気づけるようになった。

## Prev

```typescript
const key = "KEY";

class Foo {
    [key]: string;
    foo: string; // ❌ ERROR 
    
    constructor(str: string) { }
}
```

## Current

```typescript
const key = "KEY";

class Foo {
    [key]: string; // ❌ ERROR 
    foo: string; // ❌ ERROR 
    
    constructor(str: string) { }
}
```
