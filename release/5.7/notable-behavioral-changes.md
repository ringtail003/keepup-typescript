# Notable Behavioral Changes

## lib.dom.tsの変更

[https://github.com/microsoft/TypeScript/pull/60061](https://github.com/microsoft/TypeScript/pull/60061)



## TypedArrays Are Now Generic Over ArrayBufferLike

ArrayBufferの派生であるUint8Array、Int32Arrayなどが型パラメータとしてTArrayBufferを受け取るようになった。デフォルト値ArrayBufferLikeが与えられているため、従来通りパラメータなしで使用できる。

以下のようなエラーが発生した場合は `@types/node` をアップデートする。

```typescript
error ***: Type '***' is not assignable to type 'Uint8Array<ArrayBufferLike>'
```
