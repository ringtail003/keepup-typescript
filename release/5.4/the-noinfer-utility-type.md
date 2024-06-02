# The NoInfer Utility Type

## TL;DR

配列に含まれる値を示す型 `NoInfer` が追加された。

```typescript
type T1 = "red" | "green" | "blue";
type T2 = NoInfer<T1>;

const v2: T2 = "blue";
```

## Example

公式ドキュメントでは関数のシグネチャの利用例が示される。

```typescript
function f1<T extends string>(colors: T[], defaultColor: NoInfer<T>) {}

f1(["R", "G", "B"], "R"); // OK
f1(["R", "G", "B"], "orange"); // Error
```

extendsの記述に比べてシンプルで読みやすくなる。

```typescript
function f2<T extends string, T2 extends T>(colors: T[], defaultColor: T2) {}
```
