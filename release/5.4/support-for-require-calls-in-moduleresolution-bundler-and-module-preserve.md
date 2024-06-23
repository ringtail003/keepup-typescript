# Support for require() calls in --moduleResolution bundler and --module preserve

## TL;DR

`import` と `require` の両方が同じファイルで使えるようオプションが追加された。

```typescript
// tsconfig.json

"compilerOptions": {
  "module": "preserve",
  // "moduleResolution": "bundler",
  // "esModuleInterop": true,
  // "resolveJsonModule": true,
}
```

```typescript
import * as foo from "some-package/foo";
import bar = require("some-package/bar");
```

`import` と `require` の参照は `conditional exports` により決定される。

```typescript
// package.json
{
  "name": "some-package",
  "exports": {
    "./foo": {
      "import": "./esm/foo-from-import.mjs",
      "require": "./cjs/foo-from-require.cjs",
    },
    "./bar": {
      "import": "./esm/bar-from-import.mjs",
      "require": "./cjs/bar-from-require.cjs",
    },
  }
}
```
