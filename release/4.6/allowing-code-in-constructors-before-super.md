# Allowing Code in Constructors Before super()

## TL;DR

クラスのコンストラクタで最初に `super()` を呼ばないといけない縛りがなくなった。

## 前提

TypeScriptでは継承元クラスのコンストラクタを `super()` で呼び出す。

```typescript
class Bar extends Foo {
  constructor() {
    // 継承元クラスFooのコンストラクタを呼び出す
    super();
  }
}

class Foo {
  constructor() {
   // 初期処理など
  }
}
```

## Previous

最初に `super()` を呼ばなければいけないルールがあった。

{% code overflow="wrap" %}
```typescript
class Bar extends Foo {
  prop = true;

  constructor() {
    // ❌ ERROR super()が最初の呼び出しでない
    doSomething();
    super();
    
    // error: A 'super' call must be the first statement in the constructor when a class contains initialized properties, parameter properties, or private identifiers.
  }
}
```
{% endcode %}

## Current

縛りはない。

```typescript
class Bar extends Foo {
  prop = true;

  constructor() {
    // ✅ OK super()が最初の呼び出しでない
    doSomething();
    super();
    
    // OK
  }
}
```
