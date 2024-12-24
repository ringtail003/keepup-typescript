# Easier API Consumption from ECMAScript Modules

## TL;DR

esmから名前付きインポートができるようになった。

```typescript
// Before
import { foo } from "esm"; // ERROR

import * as foo from "esm";
foo.bar; // undefined

foo.default.bar; // OK
```

```typescript
// After
import { foo } from "esm"; // OK

import * as foo from "esm";
foo.bar; // not undefined
```
