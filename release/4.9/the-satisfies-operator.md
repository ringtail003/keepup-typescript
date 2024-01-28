# The satisfies Operator

```typescript
type Colors = "red" | "green" | "blue";

const colors = {
    red: "yes",
    green: false,
    blue: "kinda",
} satisfies Record<Colors, string | boolean>;
```

constとの併用も可。

```typescript
const colors = {
    ...
} as const satisfies Record<Colors, string | boolean>;
```

## 効果

### 誤った型の代入を防げる

```typescript
const g: string = colors.green;
// ERROR: Type 'boolean' is not assignable to type 'string'.
```

### \[as constがない場合] 誤った型の代入を防げる

```typescript
colors.red = true;
// ERROR: Type 'boolean' is not assignable to type 'string'.
```

### JSコードのセマンティクスを変えずにTSに移植できる

```typescript
// JS
const json = JSON.stringify({ id: 1 });

// TS: satisfiesあり
const json = JSON.stringify({ id: 1 } satisfies { id: number });

// TS: satisfiesなし
const foo: FOO = { id: 1 };
const json = JSON.stringify(foo);
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

## 参考

{% embed url="https://qiita.com/Yametaro/items/494c6e69f7e9bede2197" %}

{% embed url="https://qiita.com/suin/items/1b74645158263d2fa9af" %}
