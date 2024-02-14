# \`@satisfies\` Support in JSDoc

## TL;DR

JSファイル内で `@satisfies` が使えるようになった。

```typescript
// @ts-check

/**
 * @typedef Options
 * @prop {boolean} [strict]
 * @prop {boolean} [outDir]
 */
 
/**
 * @satisfies {Options}
 */
let options = {};
```
