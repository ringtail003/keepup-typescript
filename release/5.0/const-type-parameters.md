# \`const\` type Parameters

## TL;DR

関数の引数で `const T extends K` が使えるようになった。

## サンプル

```typescript
type Person = { names: readonly string[] };
declare function fn<const T extends Person>(arg: T): T['names'];
                    ~~~~~
                    
const foo = fn({ names: ["A", "B"] });
```

`fn()` から受け取った値は `readonly` になる。

```typescript
foo.push("X"); // ERROR
foo[0] = "X"; // ERROR
```

関数の中でも `readonly` になる。

```typescript
function fn<const T extends Person>(arg: T): T['names'] {
    arg.names.push("X"); // ERROR
```

## constが作用するのはオブジェクトのみ

`const` はオブジェクトのみ作用する。\
プリミティブには適用されない。

```typescript
declare function foo<const T extends string[]>(arg: T): T;
                     ~~~~~
                     
const names1 = foo(["A", "B"]);
names1.push("X"); // エラーにならない
```
