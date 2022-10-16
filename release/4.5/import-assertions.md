# Import Assertions

## TL;DR

インポートのシンタックス `assert` をサポートするようになった。

## JavaScriptのJSON読み込み

従来のJavaScriptでJSONを読み込むには、いちどファイルを読み込んだ後にパースする必要があった。\
モジュールとして読み込めるようにしたのが「Import Assertions」という仕様。

[https://github.com/tc39/proposal-import-assertions](https://github.com/tc39/proposal-import-assertions)

```typescript
import users from "./users.json" assert { type: "json" };
```

もともとは `assert` のないシンプルなシンタックスの予定だった。

```typescript
import users from "./users.json";
```

これには拡張子とは異なった意図しないコードが実行される危険がある。そこで開発者がJSONモジュールであることを示すために `assert` が追加された。

## TypeScriptでの扱い

JavaScriptの「Import Assertion」シンタックスをTypeScriptがサポートするようになった。

```json
// users.json
[
  { id: 1, name: "A" },
  { id: 2, name: "B" },
  { id: 3, name: "C" },
]
```

```typescript
import users from "./users.json" assert { type: "json" };

users.map(user => user.id);
```

TypeScriptはtypeに関与しないので、間違った指定でも通る。

```typescript
import users from "./users.json" assert { type: "jsooooooon" };
```

## typeはどう扱われるか

`assert` 通りのコンテンツかどうかをチェックするのはブラウザなど実行環境の仕事。\
違反がある場合、以下のようなエラーが出力される。

```typescript
// type:jsonかつ読み込んだコンテンツがJavaScriptだった時
Failed to load module script: Expected a JSON module script but the server responded 
with a MIME type of "application/javascript".
```

```typescript
// type指定がなく読み込んだコンテンツがJSONだった時
Failed to load module script: Expected a JavaScript module script 
but the server responded with a MIME type of "application/json".
```
