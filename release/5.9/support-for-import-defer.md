# Support for import defer

## TL;DR

ECMASCriptのimport defer構文をサポートした。

```typescript
import defer * as feature from "./some-feature.js"
```

## Overview

deferの構文が評価された時点では、まだ実行されない。\
機能にアクセスした時点で実行される。

```typescript
import defer * as feature from "...";

// ここでは副作用は発生しない

// このタイミングで副作用が発生する
feature.specialContent;
```

そのため名前付きインポートやデフォルトインポートは利用できない。

```typescript
// Not allowed
import defer { doSomething } from "some-module";
import defer defaultExport from "some-module";
```

deferはネットワークからの読み込みを遅延させるものではない。\
defer構文を評価した時点で読み込みは発生する。\
defer構文によってアクセスするまで実行が遅延される。

```typescript
// ネットワークからの読み込み
import defer * as feature from "...";

// 実行
feature.specialContent;
```

## Usage

deferはdownleveled（古いバージョンのECMAScriptへの変換）がされない。\
ランタイム環境でdeferをサポートしている必要があり、--moduleモードのpreserveとesnextで使用できる。
