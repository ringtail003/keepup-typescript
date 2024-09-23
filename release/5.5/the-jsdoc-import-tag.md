# The JSDoc @import Tag

## TL;DR

JSDocで `@import` がサポートされた。

## example

```typescript
import * as someModule from "./some-module";
/**
 * @param {someModule.SomeType} myValue
 */
function doSomething(myValue) {
    // ...
}
```
