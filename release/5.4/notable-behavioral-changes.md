# Notable Behavioral Changes

## \`lib.d.ts\` Changes

DOM用に生成されたコードの変更。\
[https://github.com/microsoft/TypeScript/pull/57027](https://github.com/microsoft/TypeScript/pull/57027) 参照。

### More Accurate Conditional type Constraints

Genericsを使った型の条件分岐の誤りが修正された。

```typescript
type IsArray<T> = T extends any[] ? true : false;

function foo<U extends object>(x: IsArray<U>) {
  let first: true = x; // Error
  let second: false = x; // 以前のバージョンでエラーが検出されなかった
}
```

### More Aggressive Reduction of Intersections Between type Variables and Primitive Types

交差型がより限定した推論をするようになった。　

```typescript
declare function intersect<T, U>(x:T, y:U): T & U;
```

### Improved Checking Against Template Stringswith Interpolations

テンプレートのplaceholder slotsの補完チェックが正確になった。

```typescript
function a<T extends { id: string }>() {
  let x: `-${keyof T & string}`;
  x = "-id"; // Error
}
```

### Error When Type-Only Imports Conflict with Local Values

importした型とローカル変数の名前が競合した時、エラーが検出されるようになった。

```typescript
import { Something } from "./some/path";

let Something = 123; // Error
```

### New Enum Assignability Restrictions

Enumが同じ識別子を持つ場合の互換性チェックが強化された。

```typescript
namespace First {
  export enum SomeEnum {
    A = 0,
    B = 1,
  }
}

namespace Second {
  export enum SomeEnum {
    A = 0,
    B = 2, // 値が異なる
  }
}

function foo(x: First.SomeEnum, y: Second.SomeEnum) {
  x = y; // Error
  y = x; // Error
}
```
