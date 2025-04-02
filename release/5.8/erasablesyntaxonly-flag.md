# --erasableSyntaxOnly Flag

## TL;DR

Node v23.6で、TSの型情報を削除しNode.jsから直接実行できるようになった。\
TypeScript固有のシンタックスが残るとランタイムエラーになるため、有効な構文のみで構成されていることをチェックするフラグが追加された。

フラグをオンにすると以下の構文でエラーが出力される。

```typescript
// インポートのエイリアス
import foo = require("foo");

// namespace
namespace container {}

// クラスのパラメータプロパティ
class Point {
  constructor(public x: number) {}
}

// エクスポートのアサインメント
export = Point;

// Enum
enum Direction { Up, Down, Left, Ritht }
```

TS v5.8で `--erasableSyntaxOnly`  が導入された。\
ランタイムの振る舞いを持つ、TS固有のシンタックスでエラーが検出できる。

通常このフラグは `--verbatimModduleSyntax`  と組み合わせる。

### \`--verbatimModuleSyntax\`

逐語的なトランスパイル。\
インラインでインポートしたtypeは削除され、import type文は丸ごと削除される。

```typescript
import { type A, B } from "module";
> import { B } from "module";

import type { A } from "module";
> 削除
```

オプションなしの場合、type宣言が強制されず、インポートしたものが値として使われるのか、型として使われるのか解析しないと判明しない。予期しない副作用や依存ツリーの肥大化、Babelやesbuildなどのビルドツールがシンプルに型アノテーションを削っているだけなこともあり、複雑さを生み出していた。

Ref [https://zenn.dev/teppeis/articles/2023-04-typescript-5\_0-verbatim-module-syntax](https://zenn.dev/teppeis/articles/2023-04-typescript-5_0-verbatim-module-syntax)
