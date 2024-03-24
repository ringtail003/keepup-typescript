# Stable Support \`resolution-mode\` in Import Types

## TL;DR

`import` および `<reference>` で `resolution-mode` が正式にサポートされた。\
TS v4.7以降、Night Releaseで提供されていたがStable Supportに変わる。

## use-cases

```typescript
/// <reference types="pkg" resolution-mode="require">
/// <reference types="pkg" resolution-mode="import">
```

```typescript
import { Foo } from "pkg" with {
    "resolution-mode": "require",
};

import { Bar } from "pkg" with {
    "resolution-mode": "import",
};
```
