# Private Field Presence Checks

ES2022で策定された「Ergonomic brand checks for Private Fields」をTypeScriptでサポート。

## Ergonomic brand checks for Private Fields

[https://github.com/tc39/proposal-private-fields-in-in](https://github.com/tc39/proposal-private-fields-in-in)

メソッドに渡された引数が特定のプライベートフィールドを持つかどうかを `in` 演算子で判定できる機能。

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
const shiba = new Dog("しげる");

// true
Dog.isDog(shiba);
```

「ブランドチェック」と名付けられたのは、クラス外部からアクセスできないプライベートフィールドを使った優位性にある。

## brand checks

ブランドチェックを検証してみよう。Dogと全く同じフィールドを持つCatクラスを宣言する。

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
  #name!: string;
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

この手法は「特定のプロパティを持つかどうか」を検査するため、別のインスタンスであってもプロパティを持っていれば同一とみなされる。

```typescript
function isDog(arg: any): arg is Dog {
  return (arg as Dog).name !== undefined;
}
```

```typescript
class Dog {
  #name: string;
  get name() {
    return this.#name;
  }
}

class Cat {
  #name: string;
  get name() {
    return this.#name;
  }
}
```

```typescript
const norwegianForestCat = new Cat("しげる");

// true
isDog(norwegianForestCat);
```

ブランドチェックはクラスが同じかどうか検査されるため、type predicatesと比較して優位性がある。

```typescript
// true
isDog(norwegianForestCat);

// false
Dog.isDog(norwegianForestCat);
```
