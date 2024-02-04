# Performance Improvements

* TS内部の `forEachChild` をリファクタしたことでLanguage Serviceのパフォーマンスが向上した。
* Conditional Typeの `T extends T2` 評価を内部で遅延させることで型チェックのパフォーマンスが向上した。
