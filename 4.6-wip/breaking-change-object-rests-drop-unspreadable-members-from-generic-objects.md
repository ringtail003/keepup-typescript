# BREAKING CHANGE:Object Rests Drop Unspreadable Members from Generic Objects

## TL;DR

分割代入＆スプレッド構文を使った推論が修正された。

## Previous

{% code title="foo.ts" %}
```typescript
class Thing {
  prop = 42;
  method() {}
}

function foo(thing: Thing) {
  let { prop, ...rest } = thing;
  rest.someMethod();
}

foo(new Thing());
```
{% endcode %}

トランスパイル後のJSでエラーになる。

{% code title="dist/foo.js" %}
```javascript
class Thing {
    prop = 42;
    method() {}
}
function foo(thing) {
    let { prop, ...rest } = thing;
    rest.someMethod(); // ❌ ERROR! rest === {}
}

foo(new Thing());
```
{% endcode %}

## Current

TSでエラーが表示されるようになった。

<pre class="language-typescript"><code class="lang-typescript">class Thing {
  prop = 42;
  method() {}
}

function foo(thing: Thing) {
<strong>  let { prop, ...rest } = thing; // ❌ ERROR!
</strong>  rest.someMethod();
}

foo(new Thing());
</code></pre>
