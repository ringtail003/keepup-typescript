# Group-Aware Organize Imports

## TL;DR

Organize Importが改善されグループが考慮されるようになった。

## 例

以下のコードをOrganize Importで並べ替えるとする。

```typescript
// local code
import * as bbb from "./bbb";
import * as ccc from "./ccc";
import * as aaa from "./aaa";

// built-ins
import * as path from "path";
import * as child_process from "child_process"
import * as fs from "fs";
```

以下のようになっていた。

```typescript
// local code
import * as child_process from "child_process";
import * as fs from "fs";
// built-ins
import * as path from "path";
import * as aaa from "./aaa";
import * as bbb from "./bbb";
import * as ccc from "./ccc";
```

改善後はこうなる。\`\`い
