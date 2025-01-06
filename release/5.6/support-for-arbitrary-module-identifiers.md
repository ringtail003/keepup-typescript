# Support for Arbitrary Module Identifiers

JSでは文字列リテラルでエクスポートができる。

```typescript
const banana = "🍌";
export { banana as "🍌" };
```

それを識別子としてインポートできる。

```typescript
import { "🍌" as banana } from "./banana";

const fn = (value: string) {}

fn(banana);
```

TSでこのような構文がサポートされるようになった。\
他言語運用やesbuildなどでの有用性がある、とドキュメントに書いてある。
