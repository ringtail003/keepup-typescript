# module es2022

## TL;DR

コンパイラオプション `module`で `es2022` が指定できるようになった。メジャーな機能は「top-level await」。

## moduleとは何ぞ

tsconfig.jsonまたは `--module ***` オプションで指定ができる。

{% tabs %}
{% tab title="tsconfig.json" %}
```javascript
"module": "es2022"
```
{% endtab %}
{% endtabs %}

`module` とは、どのようなモジュールパターンの環境で使われるかを指定するもの。\
指定によってトランスパイル後のJavaScriptに変化が生じる。

{% tabs %}
{% tab title="module:"es2022"" %}
```javascript
import { valueOfPi } from "./constants";
export const twoPi = valueOfPi * 2;
```
{% endtab %}

{% tab title="module:"CommonJS"" %}
```javascript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
exports.twoPi = void 0;
const constants_1 = require("./constants");
exports.twoPi = constants_1.valueOfPi * 2;
```
{% endtab %}

{% tab title="module:"umd"" %}
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

今回のアップデートで `module` に `es2022` が指定できるようになった。

## top-level awaitを使ってみる

`module:es2022` を設定するとtop-level awaitが使える。

<figure><img src="../../.gitbook/assets/スクリーンショット 2022-10-10 23.29.49.png" alt=""><figcaption></figcaption></figure>

`module:es2020` など古いバージョンではtop-level awaitは使えない。

<figure><img src="../../.gitbook/assets/スクリーンショット 2022-10-10 23.32.48.png" alt=""><figcaption></figcaption></figure>

