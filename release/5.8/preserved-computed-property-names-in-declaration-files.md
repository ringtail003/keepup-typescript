# Preserved Computed Property Names in Declaration Files

## TL;DR

クラスのメンバ名で、stringのリテラルが許容されるようになった。

```typescript
let propName = "foo";

class Foo {
  [propName]: number;
}
```

`--isolatedDeclarations`  フラグを使うと上記シンタックスはエラーになる。
