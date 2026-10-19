# The dom lib Now Contains dom.iterable and dom.asynciterable

## Overview

tsconfig.json の `lib:dom` にはイテレータ系が含まれていなかった。\
querySelectorAll など使う時は `dom.iterable` を別指定する必要があったが v6.0 から `dom` に統合された。
