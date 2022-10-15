---
description: 型のインポート・エクスポートで新しいシンタックスが使えるようになった。
---

# type Modifiers on Import Names

TS3.8で導入された「Type-Only Imports and Export」で型のみインポート・エクスポートできるようになった。

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

型と値の両方をインポートするには、それぞれ別に `import` する必要があった。

```typescript
import type { User } from "./user"; // <=== 型のインポート
import { isUser } from "./user"; // <=== 値（関数）のインポート

const user: User = {
  id: 1,
  nickname: "user1",
};
isUser(user);
```

TS4.5で新しいシンタックスが導入され、1回の `import` で済むようになった。

```typescript
// 以前のバージョン
import type { User } from "./user";
import { isUser } from "./user";
```

```typescript
// TS4.5
import { type User, isUser } from "./user";
```
