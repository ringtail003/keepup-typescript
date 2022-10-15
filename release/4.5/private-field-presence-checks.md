# Private Field Presence Checks

ES2022で策定された「Ergonomic brand checks for Private Fields」をTypeScriptでサポート。

## Ergonomic brand checks for Private Fields

[https://github.com/tc39/proposal-private-fields-in-in](https://github.com/tc39/proposal-private-fields-in-in)

メソッドに渡された引数が特定のプライベートフィールドを持つかどうかを `in` 演算子で判定できる機能。

{% tabs %}
{% tab title="Dog" %}
```typescript
class Dog {
  #name!: string;

  constructor(name: string) {
    this.#name = name;
  }

  static isDog(arg: any) {
    return #name in arg; // <=== これ！
  }
}
```
{% endtab %}
{% endtabs %}

```typescript
const shiba = new Dog("しげる");

// true
Dog.isDog(shiba);
```

「ブランドチェック」と名付けられたのは、クラス外部からアクセスできないプライベートフィールドを使った優位性にある。

## brand checks

ブランドチェックを検証してみよう。Dogと全く同じフィールドを持つCatクラスを宣言する。

{% tabs %}
{% tab title="Cat" %}
```typescript
class Cat {
  #name!: string;

  constructor(name: string) {
    this.#name = name;
  }

  static isCat(arg: any) {
    return #name in arg;
  }
}
```
{% endtab %}

{% tab title="Dog" %}
```typescript
class Dog {
  #name!: string;

  constructor(name: string) {
    this.#name = name;
  }

  static isDog(arg: any) {
    return #name in arg;
  }
}
```
{% endtab %}
{% endtabs %}

```typescript
const norwegianForestCat = new Cat("しげる");

// true
Cat.isCat(norwegianForestCat);
```

DogのしげるとCatのしげるは名前が同じでも、それぞれ別のクラスのインスタンスである。

```typescript
// false
Dog.isDog(norwegianForestCat);

// false
Cat.isCat(shiba);
```

argにCatのインスタンスが渡された時、Cat#nameにアクセスできないのでfalseが返る。

```typescript
class Dog {
  #name!: string;bs
  ...
  static isDog(arg: any) {
    return #name in arg;
  }
```

Dog#nameにアクセスできるのはDogだけ。\
argにDogのインスタンスが渡された時、Dog#nameにアクセスできるのでtrueが返る。

クラスのブランドが担保できることから「ブランドチェック」と名付けられた。

## type predicates

TypeScriptには「type predicates」という型の同一性を検査する手法がある。

[https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates)

この手法は「特定のプロパティを持つかどうか」を検査する。

```typescript
function isDog(arg: any): arg is Dog {
  return (arg as Dog).name !== undefined;
}
```

{% tabs %}
{% tab title="Dog" %}
```typescript
class Dog {
  #name: string;
  
  constructor(name: string) {
    this.#name = name;
  }

  static isDog(arg: any) {
    return #name in arg;
  }
  
  get name() {
    return this.#name;
  }
}
```
{% endtab %}

{% tab title="Cat" %}
```typescript
class Cat {
  #name: string;

  constructor(name: string) {
    this.#name = name;
  }

  static isDog(arg: any) {
    return #name in arg;
  }
  
  get name() {
    return this.#name;
  }
}
```
{% endtab %}
{% endtabs %}

別のインスタンスであってもプロパティを持っていれば同一とみなされる。

```typescript
const norwegianForestCat = new Cat("しげる");

// true
isDog(norwegianForestCat);
```

ブランドチェックはクラスが同じかどうか検査されるため、type predicatesと比較して優位性がある。

```typescript
// type predicates: true
isDog(norwegianForestCat);

// brand checks: false
Dog.isDog(norwegianForestCat);
```
