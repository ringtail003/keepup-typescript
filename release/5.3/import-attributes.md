# Import Attributes

## TL;DR

ECMAScriptの `Import Attributes` 構文をサポートした。

[https://app.gitbook.com/o/UdQ1oyvbJoi9eJ7Jfkiq/s/vnW4VLmDDZnkg6kDdouY/proposal/import-attribute](https://app.gitbook.com/s/vnW4VLmDDZnkg6kDdouY/proposal/import-attribute)

```typescript
import obj from "./foo.json" with { type: "json" };
```

`with` はJSで扱われるメタデータ。\
TSは特別なことは行わず、処理はブラウザなどランタイム環境に委譲する。

TS4.5で Import Assertion がサポートされたが、ECMAScriptでImport Attributesとしてアップデートされたことにより古い構文となった。TSでも将来的に古い構文は廃止される。

```typescript
// v4.5: Legacy
import obj from "./foo.json" assert { type: "json" };
```
