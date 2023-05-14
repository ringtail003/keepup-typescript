# JSDoc Name Suggestions

## TL;DR

JSDocで記述されたパラメータが存在しない場合にエラーが得られるようになった。

## サンプルコード

```javascript
/**
 * @param param1 {string} first argument
 * @param param2 {string} second argument
 */
function foo(param1) {
}
```

JavaScriptをトランスパイル対象にするために以下のオプションをセットする。

{% code title="tsconfig.json" %}
```typescript
{
  "compilerOptions": {
    "allowJs": true,
    "checkJs": true,
  }
}
```
{% endcode %}

## Previous

エラーなし。

## Current

```javascript
/**
 * @param param1 {string} first argument
 * @param param2 {string} second argument // ❌ ERROR!
 */
function foo(param1) {
}
```
