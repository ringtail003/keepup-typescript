# Quick Fix for Adding Missing Parameters

## TL;DR

関数呼び出しで多すぎる引数を指定した時、Quick Fixが表示されるようになった。

```typescript
declare function foo(a:string);

foo(a); // OK
foo(a, b, c); // Quick Fix
```
