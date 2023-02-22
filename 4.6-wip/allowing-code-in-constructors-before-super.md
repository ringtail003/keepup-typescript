# Allowing Code in Constructors Before super()

## TL;DR

クラスのコンストラクタで最初に `super()` を呼ばないといけない縛りがなくなった。

## Previously

最初に `super()` を呼ばなければいけない。

{% code overflow="wrap" %}
```typescript
class Bar extends Foo {
  prop = true;

  constructor() {
    // error: A 'super' call must be the first statement in the constructor when a class contains initialized properties, parameter properties, or private identifiers.
    doSomething();
    super();
  }
}
```
{% endcode %}

## v4.6+

縛りはない。

```typescript
class Bar extends Foo {
  prop = true;

  constructor() {
    doSomething();
    super();
  }
}
```
