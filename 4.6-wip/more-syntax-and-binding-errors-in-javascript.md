# More Syntax and Binding Errors in JavaScript

## TL;DR

JSのシンタックスエラーが強化された？

内容がよく掴めなかったので、オプション指定によるJSチェックを整理。

## サンプルのJSコード

{% code title="some.js" %}
```javascript
function container() {
  export function foo() {}
}

const a = 123;
const a = 456;
```
{% endcode %}

## allowJs

falseの場合、JSは吐き出されない。

<pre class="language-typescript" data-title="tsconfig.json"><code class="lang-typescript">{
  "compilerOptions": {
<strong>    "allowJs": false,
</strong>  }
}
</code></pre>

<pre class="language-bash"><code class="lang-bash">$ tsc

├── dist
<strong>│   └── 
</strong>├── src
│   └── a.js
</code></pre>

trueの場合、JSが吐き出される。

<pre class="language-typescript" data-title="tsconfig.json"><code class="lang-typescript">{
  "compilerOptions": {
<strong>    "allowJs": true,
</strong>  }
}
</code></pre>

<pre class="language-bash"><code class="lang-bash">$ tsc

├── dist
<strong>│   └── a.js
</strong>├── src
│   └── a.js
</code></pre>

## checkJs

falseの場合、JSのエラーは検出しない。

<pre class="language-typescript" data-title="tsconfig.js"><code class="lang-typescript">{
  "compilerOptions": {
<strong>    "checkJs": false,
</strong>  }
}
</code></pre>

{% code title="src/a.js" %}
```typescript
function container() {
  export function foo() {}
}

const a = 123;
const a = 456;
```
{% endcode %}

trueの場合、JSのエラーが検出される。

<pre class="language-typescript" data-title="tsconfig.json"><code class="lang-typescript">{
  "compilerOptions": {
<strong>    "checkJs": true,
</strong>  }
}
</code></pre>

{% code title="src/a.js" %}
```typescript
function container() {
  export function foo() {} // ❌ ERROR!
}

const a = 123;
const a = 456; // ❌ ERROR!
```
{% endcode %}

## @ts-check

checkJsがfalseの時、特定のJSファイルだけエラー検出対象にする

```typescript
// @ts-check
function container() {
  export function foo() {} // ❌ ERROR!
}

const a = 123;
const a = 456; // ❌ ERROR!
```

## @ts-nocheck

checkJsがtrueの時、特定のJSファイルだけエラーを検出しない

```typescript
// @ts-nocheck
function container() {
  export function foo() {}
}

const a = 123;
const a = 456;
```
