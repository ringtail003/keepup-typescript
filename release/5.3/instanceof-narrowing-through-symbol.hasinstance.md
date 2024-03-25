# \`instanceof\` Narrowing Through \`Symbol.hasInstance\`

## TL;DR

`instanceof` の型が絞り込まれるようになった。

## use-cases

JSで `instanceof` をカスタム実装できる。

```typescript
class Foo {
    static [Symbol.hasInstance](value) {
        return true;
    }
}
```

以前のバージョンではNarrowingが効かないケースがあったが解消した。

{% code overflow="wrap" %}
```typescript
interface Foo {}

class Bar implements Foo {
    bar = "";
    method() {}
    static [Symbol.hasInstance](value): value is Foo {}
}

if (x instanceof Foo) {
    x.bar; // OK
    x.method(); // v5.3 ERROR: Property 'method' does not exist on type 'Foo'.
}
```
{% endcode %}
