# The satisfies Operator

```typescript
type Colors = "red" | "green" | "blue";

const colors = {
    red: "yes",
    green: false,
    blue: "kinda",
} as const satisfies Record<Colors, string | boolean>;
```

## オブジェクト値の型を推論できる

```typescript
const g: string = colors.green;
// ERROR: Type 'boolean' is not assignable to type 'string'.
```

## \[as constがない場合] 誤った型の代入を防げる

```typescript
colors.red = true;
// ERROR: Type 'boolean' is not assignable to type 'string'.
```

## \[as constがある場合] 書き換えを防止する

```typescript
colors.red = "magenda";
// ERROR: Cannot assign to 'red' because it is a read-only property.
```

## 従来の書き方

```typescript
const colors: { [P in Colors]: string | boolean } = {
    red: "yes",
    green: false,
    blue: "kinda",
};

// 型推論：OK
const g: string = colors.green;

// 誤った型の代入：NG
colors.red = true;

// 書き換え防止：NG
colors.red = "lime";

```

