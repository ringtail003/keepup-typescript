# resolution-mode

## TL;DR

トリプルスラッシュ・ディレクティブにモジュール解決モードを指定できるようになった。

## 例

以下のように記述する。

```typescript
/// <reference types="pkg" resolution-mode="require" />
/// <reference types="pkg" resolution-mode="import" />
```

import構文は以下のように記述する。（Nightly Buildでサポート）

```typescript
import type { TypeFromRequire } from "pkg" assert {
    "resolution-mode": "require"
};

import type { TypeFromImport } from "pkg" assert {
    "resolution-mode": "import"
};
```

import構文は以下のように記述する。（Nightly Buildでサポート）
