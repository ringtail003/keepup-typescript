# Path Rewriting for Relative Paths

## TL;DR

`--rewriteRelativeImportExtensions` フラグで `import {} as "./foo.ts"` を `import {} as "./foo.js"` に書き換えるようになった。

TSをビルド無しで読み込んで実行するin-placeツールが増えた（ts-node, Deno, Bunなど）。\
Node.jsでも `--experimental-strip-types` フラグでTSを実行できるようになった。\
ライブラリ作者は、開発環境では `.ts` を実行し、ビルド時に `.js` に変換する必要が出てきた。



書き換えの条件

* `--rewriteRelativeImportExtensions` フラグあり
* `./` や `../` から始まる相対パス
* インポートの拡張子が `.ts` `.tsx` `.mts` `.cts`

```typescript

// 書き換えられる
import * as foo from "./foo.ts";
import * as bar from "../someFolder/bar.mts";

// 書き換えられない
import * as a from "./foo";
import * as b from "some-package/file.ts";
import * as c from "@some-scope/some-package/file.ts";
import * as d from "#/file.ts";
import * as e from "./file.js";
```



公式ドキュメントには、インポートするファイル名が動的に決定するケースや、package.jsonの `imports` `exports` を使ったケースについて説明があるが割愛。
