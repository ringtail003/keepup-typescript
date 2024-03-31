# Settings to Prefer \`type\` Auto-Imports

## TL;DR

エディタの自動インポートで `import { type X }` がサポートされるようになった。

```typescript
// Prev
import { Person from "./types";

// Current
import { type Person } from "./types";

export let p: Person;
```

エディタの設定で `typescript.preferences.preferTypeOnlyAutoImports` を有効にする必要がある。
