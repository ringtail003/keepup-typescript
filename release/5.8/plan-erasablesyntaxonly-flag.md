# plan: --erasableSyntaxOnly Flag

## TL;DR

Node v23.6〜型情報を削除しTSファイルを直接実行できるようになった。\
TypeScript固有のシンタックスが残るとランタイムエラーになるため、有効な構文のみで構成されていることをチェックするフラグが追加された。

フラグをオンにすると以下の構文でエラーが出力される。

* enum
* namespace
* importのエイリアス
* classのパラメータプロパティ `constructor(private x: number)`\
