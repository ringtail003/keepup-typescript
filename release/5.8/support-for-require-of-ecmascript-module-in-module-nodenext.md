# Support for require() of ECMAScript Module in \`--module nodenext\`

## TL;DR

.node v22で `require("esm")`  ができるようになった。\
（トップレベルのawaitを含むファイルを除く。）\
パッケージ開発者がCommonJSとESMのファイルを並行して公開しなくても、相互運用できるようになる。

* ESMでCommonJSを読み込む `import foo from "cjs"`
* CommonJSでESMを読み込む `require("esm")`

TSでは `require("esm")`  をエラーとして検出していたが、v5.8から `--module nodenext`  フラグを付与すると検出しなくなる。
