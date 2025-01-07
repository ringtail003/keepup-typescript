# Exclude Patterns for Auto-Imports

## TL;DR

パッケージごとに自動インポートする範囲をエディタのオプションで指定できるようになった。

```typescript
// lodash配下すべて
"typescript.preferences.autoImportSpecifierExcludeRegexes": [
    "^lodash/.*$"
]

// エントリポイントのみ
"typescript.preferences.autoImportSpecifierExcludeRegexes": [
    "^lodash$"
]

// nodeのインポートを回避
"typescript.preferences.autoImportSpecifierExcludeRegexes": [
    "^node:"
]
```

.`i` aaaa
