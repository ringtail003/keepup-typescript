# Resolution Customization with moduleSuffixes

## TL;DR

ファイル名のサフィックスをモジュール解決に利用できるようになった。

## 例

moduleSuffixesに `.ios` `.native` を指定する。

{% code title="tsconfig.json" %}
```typescript
"compilerOptions": {
    "moduleSuffixes": [".ios", ".native", ""]
}
```
{% endcode %}

`.ts` ファイルでインポートを書く。

{% code title="bar.ts" %}
```typescript
import * as foo from "./foo";
```
{% endcode %}

TSコンパイラは以下の順でファイルを探索する。

* `./foo.ios.ts`
* `./foo.native.ts`
* `./foo.ts`

## ユースケース

Reactで複数プラットフォームの分岐が想定されている。
