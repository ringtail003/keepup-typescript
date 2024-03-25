# Checks for \`super\` Property Access on Instance Fields

## TL;DR

`super` の使い方が厳密にチェックされるようになった。

## use-case

```typescript
class Foo {
    fooMethod() {}
}

class Bar extends Foo {
    constructor() {
        super();
        super.fooMethod(); // ERROR
        this.fooMethod(); // OK
    }
    
    barMethod() {
        super.barMethod(); // ERROR
        this.fooMethod(); // OK
    }
}
```
