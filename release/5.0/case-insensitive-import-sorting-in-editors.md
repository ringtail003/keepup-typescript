# Case-Insensitive Import Sorting in Editors

## TL;DR

`import` 文のソートで、大文字・小文字に関わらずASCII順で並ぶようになった。

```typescript
import {
    a,
    A,
    b,
    C
} from "";
```

VSCodeの設定でソートのオプション指定ができる。

```typescript
typescript.unstable: {
    organizeImportsIgnoreCase: "auto",
    organizeImportsCollation: "unicode",
    organizeImportsLocale: "en",
    organizeImportsCaseFirst: "upper",
    organizeImportsNumericCollation: true,
},
javascript.unstable: {
    // typescriptと同じ指定
}
```
