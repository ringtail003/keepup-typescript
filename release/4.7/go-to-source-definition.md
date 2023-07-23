# Go to Source Definition

## TL;DR

エディタで宣言（d.ts）より先の実装（.jsや.cjs）にジャンプできるようになった。

## 例

Visual Studio Codeのコンテキストメニューで「Go to Source Implementation」が選択できる。

<figure><img src="../../.gitbook/assets/スクリーンショット 2023-07-23 12.23.45.png" alt=""><figcaption></figcaption></figure>

RxJSのコードで試してみた。\
このライブラリはJSで書かれたもので、d.tsファイルで型が補完されている。

Go to Definitionの場合 **Observable.d.ts** にジャンプする。

<figure><img src="../../.gitbook/assets/スクリーンショット 2023-07-23 12.25.36.png" alt=""><figcaption></figcaption></figure>

Go to Source Definitionだと **Observable.js** にジャンプする。

<figure><img src="../../.gitbook/assets/スクリーンショット 2023-07-23 12.26.18.png" alt=""><figcaption></figcaption></figure>
