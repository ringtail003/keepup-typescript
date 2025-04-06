# The \`--lib Replacement\` Flag

## TL;DR

TS4.5で導入された `lib`  の差し替えはnode\_modulesの監視をともなう。\
TS5.8で監視を止めるオプションとして `--libReplacement`  が導入された。

```typescript
// package.json
{
  "devDependencies": {
    "@typescript/lib-dom": "npm:@types/web@0.0.199"
  }
}
```

```bash
# Default
--libReplacement true

# 監視を止める（NEW）
--libReplacement false
```
