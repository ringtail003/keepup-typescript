# Inline Variable Refactoring

## TL;DR

変数をインライン展開するエディタのコマンド `Inline variable` が追加された。

```typescript
// before
const path = ".some_file";
const file = fs.openSync(path, "rw");

// Inline variable

// after
const file = fs.openSync(".some_file", "rw");
```
