# type Modifiers on Import Names

## TL;DR

型のインポート・エクスポートで新しいシンタックス `import { type Foo }` が使えるようになった。

## Type-Only Imports and Export（V3.8）

「Type-Only Imports and Export」というシンタックスを使うと、型のみインポート・エクスポートできる。

[https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#type-only-imports-and-export](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#type-only-imports-and-export)

```typescript
// user.ts
export type User = {
  id: number;
  nickname: string;
};

export function isUser(arg: any): arg is User {
  return arg.id !== undefined && arg.nickname !== undefined;
}
```

```typescript
import type { User } from "./user"; // <=== 型のみインポート

const user: User = {
  id: 1,
  nickname: "user1",
};
```

型と値の両方をインポートする場合、それぞれ別に `import` する必要がある。

```typescript
import type { User } from "./user"; // <=== 型のインポート
import { isUser } from "./user"; // <=== 値（関数）のインポート

const user: User = {
  id: 1,
  nickname: "user1",
};
isUser(user);
```

今回のアップデートで新しいシンタックスが導入され、1回の `import` で済むようになった。

```typescript
// 以前のバージョン
import type { User } from "./user";
import { isUser } from "./user";
```

```typescript
// V4.5
import { type User, isUser } from "./user";
```
