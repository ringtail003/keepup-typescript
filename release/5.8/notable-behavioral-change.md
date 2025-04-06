# Notable Behavioral Change

## lib.d.ts

[https://github.com/microsoft/TypeScript/pull/60987](https://github.com/microsoft/TypeScript/pull/60987)\


## Restrictions on Import Assertions Under \`--module nodenext\`

ECMAScriptで提案されたimport assertionは、議論の過程でimport attributesに代わった。\
Node.js v22はassertシンタックスを許容しない。\
TS5.8で `--module nodenext`  を使うとassertシンタックスをエラーとして検出する。

```typescript
// ERROR: ランタイムとの互換性がない
import data from "./data.json" assert { type: "json" }

// ✅ 
import data from "./data.json" with { type: "json" }
```
