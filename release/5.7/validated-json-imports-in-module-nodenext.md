# Validated JSON Imports in --module nodenext

## TL;DR

`--module nodenext` を有効にした時にjsonタイプを指定して読み込めるようになった。

```typescript
import foo from "./a.json" with { type: "json" };
```

jsonに対してnamed exportsをしないため、defaultを通してアクセスする。

```typescript
import foo from "./a.json" with { type: "json" };

let version = foo.default.version;
```
