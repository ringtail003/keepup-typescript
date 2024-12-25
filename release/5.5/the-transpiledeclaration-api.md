# The transpileDeclaration API

## TL;DR

v5.5以前の `transpileModule`と同じ仕様の `transpileDeclation` APIが追加された。\
transpileModuleはisolateModuleに違反するエラーがあると正しく出力されない問題があったが、transpileModuleで解消された。
