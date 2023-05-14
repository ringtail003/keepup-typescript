# --target es2022

## TL;DR

`--target es2022` が指定できるようになった。

## サンプルコード

以下のコードを `es5` と `es2022` それぞれでトランスパイルした結果を比較する。

```typescript
class Foo {
  private className = 'FOO';
}

function foo() {
  return [0, 1, 2].at(0);
}
```

## --target es5

<pre class="language-typescript" data-title="tsconfig.json"><code class="lang-typescript">{
  "compilerOptions": {
<strong>    "target": "es5",
</strong>  }
}
</code></pre>

Array.atはそもそもコンパイルエラーになる。

```typescript
return [0, 1, 2].at(0);
// ❌ ERROR! Property 'at' does not exist on type 'number[]'.
```

トランスパイル後のJavaScriptは以下のようになる。

```javascript
"use strict";
var Foo = /** @class */ (function () {
    function Foo() {
        this.className = 'FOO';
    }
    return Foo;
}());
function foo() {
    return [0, 1, 2].at(0);
}
```

## --target es2022

<pre class="language-typescript" data-title="tsconfig.json"><code class="lang-typescript">{
  "compilerOptions": {
<strong>    "target": "es2022",
</strong>  }
}
</code></pre>

トランスパイル後のJavaScriptは以下のようになる。

```typescript
"use strict";
class Foo {
    constructor() {
        this.className = 'FOO';
    }
}
function foo() {
    return [0, 1, 2].at(0);
}

```
