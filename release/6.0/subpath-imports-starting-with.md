# Subpath Imports Starting with \`#/\`

## Overview

Node.jsのSubpath imports, Subpath patternsに追随するもの。

[https://nodejs.org/api/packages.html#subpath-imports](https://nodejs.org/api/packages.html#subpath-imports)

これまで扱えなかった imports の `#/` を使えるようになった。

```
// package.json
{
  "type": "module",
  "imports": {
    "#/*": "./dist/*"
  } 
}
```

```
import utils from "#/utils.js";
```

## Previous

Subpath imports は長いインポート文を避けるためのもの。\
内部エイリアス（プロジェクト内で完結し、ライブラリとして公開した時に意味を持たない）として作用する。

これまでは `#/*` という書き方ができず `#root/*` と書く必要があったが、より短い構文が使えるようになった。

```
// package.json
{
  "imports": {
    "#root/*": "./dist/*"
  }
}
```

```
import utils from "#root/utils.js";
```

