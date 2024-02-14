# --verbatimModuleSyntax

## TL;DR

importがビルドJSで失われないようにするオプション。

```typescript
import type { A } from "x";

// JS
// (empty)
```

```typescript
import { a, type A } from "x";

// JS
import { a } from "x";
```

```typescript
import { type A } from "x";

// JS
import {} from "x";
```

## 補足

通常、値のインポート以外はビルドJSに出力されない。\
この挙動がESModuleの挙動を破壊することがある。

`--importsNotUsedAsValues` `--preserveValueImports` `--isolatedModules` など複数のオプションを利用して調整する必要があったが、シンプルに指定するための `--verbatimModuleSyntax` が導入された。

`{ }` で囲まれていない型インポートはJSに出力されない。

```typescript
import type * as a from "x";

import { type A } from "x";
export { type A } from "x";

// JS
// (empty)
```
