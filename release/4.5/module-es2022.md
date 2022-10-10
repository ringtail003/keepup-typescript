# module es2022

コンパイラオプション `module`で `es2022` が指定できるようになった。メジャーな機能は「top-level await」。

## moduleとは何ぞ

tsconfig.jsonまたは `--module exnext` オプションで指定ができる。

{% tabs %}
{% tab title="tsconfig.json" %}
```javascript
"module": "es2022"
```
{% endtab %}
{% endtabs %}

どのようなモジュールパターンの環境で使われるか（Node.jsなら `CommonJS` 、ブラウザなら `umd` や `es****` など）に合わせて指定するもの。

{% tabs %}
{% tab title="ES2020" %}
```javascript
import { valueOfPi } from "./constants";
export const twoPi = valueOfPi * 2;
```
{% endtab %}

{% tab title="CommonJS" %}
```javascript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
exports.twoPi = void 0;
const constants_1 = require("./constants");
exports.twoPi = constants_1.valueOfPi * 2;
```
{% endtab %}

{% tab title="UMD" %}
```javascript
(function (factory) {
    if (typeof module === "object" && typeof module.exports === "object") {
        var v = factory(require, exports);
        if (v !== undefined) module.exports = v;
    }
    else if (typeof define === "function" && define.amd) {
        define(["require", "exports", "./constants"], factory);
    }
```
{% endtab %}
{% endtabs %}

参考： [https://qiita.com/hareku/items/dbf0752aa76499a895fd](https://qiita.com/hareku/items/dbf0752aa76499a895fd)

## TypeScript PlayGround

「TS Config」で `module:es2022` とすると、確かにエラーが出なくでなくなっている。

<figure><img src="../../.gitbook/assets/スクリーンショット 2022-10-10 23.29.49.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/スクリーンショット 2022-10-10 23.32.48.png" alt=""><figcaption></figcaption></figure>

