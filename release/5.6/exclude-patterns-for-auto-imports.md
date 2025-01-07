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

`i` や `u` フラグを使う時はスラッシュをエスケープする。

```typescript

"typescript.preferences.autoImportSpecifierExcludeRegexes": [
    "^./lib/internal",        // no escaping needed
    "/^.\\/lib\\/internal/",  // escaping needed - note the leading and trailing slashes
    "/^.\\/lib\\/internal/i"  // escaping needed - we needed slashes to provide the 'i' regex flag
]
```
