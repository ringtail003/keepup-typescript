# Auto-Accessors in Classes

## TL;DR

ECMAScriptのauto-accessorがサポートされた。

```typescript
class Person {
    accessor name: string = "";
}
```

## TC39 Proposal

[https://github.com/tc39/proposal-grouped-and-auto-accessors](https://github.com/tc39/proposal-grouped-and-auto-accessors)

auto-accessorはgetter/setterをかんたんに記述する構文。

```typescript
class Person {
    accessor x {
        get() {}
        set() {}
    }
    accessor y {
        get() {}
        #set() {}
    }
    static accessor z {
        get() {}
        set() {}
    }
}
```

v4.9では冒頭の構文のみサポートされ、他の構文はエラーになった。\
public変数と変わらないのでは？
