# BREAKING CHANGE:JavaScript Files Always Receive Grammar and Binding Errors

## TL;DR

JSファイルのチェックが強化された。

## Previous

例えば重複した宣言など、以前のバージョンではエラーにならなかった。

{% code title="foo.js" %}
```javascript
const a = 123;
const a = 123;
```
{% endcode %}

## Current

エラーが検出されるようになった。

{% code title="foo.js" %}
```javascript
const a = 123;
const a = 123; // ❌ ERROR! 
```
{% endcode %}
