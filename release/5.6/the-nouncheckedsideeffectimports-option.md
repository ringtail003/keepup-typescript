# The --noUncheckedSideEffectImports Option

## TL;DR

フラグをONにすることで、side effect imports（Prototypeのメソッドやグローバル変数の暗黙的な取り込み）でパスが存在するかどうかチェックされるようになった。

```typescript
// tsconfig.json
noUncheckedSideEffectImports: true
```

{% code overflow="wrap" %}
```typescript
import "some-module";

// < v5.6
// インポートが無視される

// v5.6+
// error: Cannot find module 'some-module' or its corresponding type declarations.
```
{% endcode %}

特定のファイル名をチェック対象外としたい時は、アンビエントモジュール宣言を追加する。

```typescript
// global.d.ts
declare module "*.css" {}
```
