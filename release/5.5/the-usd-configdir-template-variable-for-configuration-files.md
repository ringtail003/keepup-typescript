# The ${configDir} Template Variable for Configuration Files

## TL;DR

`tsconfig.base.json` から派生した `tsconfig.json` のディレクトリを基準としたパス指定ができるようになった。

### Before

相対パス：extendsされた派生元が基準

```typescript
// tsconfig.base.json
outDir: "./dist"

// packages/foo/tsconfig.json
extends: "../../tsconfig.base.json"

// 出力されるディレクトリ：dist
```

### After

configDir：extendsした派生先が基準

```typescript
// tsconfig.base.json
outDir: "${configDir}/dist"

// packages/foo/tsconfig.json
extends: "../../tsconfig.base.json"

// 出力されるディレクトリ：packages/foo/dist
```
