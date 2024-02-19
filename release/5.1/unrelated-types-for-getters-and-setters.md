# Unrelated Types for Getters and Setters

## TL;DR

setterで共有型を返せるようになった。

## Current

```typescript
class Foo {
    #value: string | boolean;
    
    set value(v: string) {}
    get value(): string | undefined; // これ
}
```
