# Notable Behavioral Changes

## lib.dom.tsの変更

[https://github.com/microsoft/TypeScript/pull/60061](https://github.com/microsoft/TypeScript/pull/60061)



## TypedArrays Are Now Generic Over ArrayBufferLike

ArrayBufferの派生であるUint8Array、Int32Arrayなどが型パラメータとしてTArrayBufferを受け取るようになった。デフォルト値ArrayBufferLikeが与えられているため、従来通りパラメータなしで使用できる。

以下のようなエラーが発生した場合は `@types/node` をアップデートする。

```typescript
error ***: Type '***' is not assignable to type 'Uint8Array<ArrayBufferLike>'
```

## Creating Index Signatures from Non-Literal Method Names in Classes

.従来はクラスのメンバ関数がSymbolで与えれている場合に、TS内部でindex signatureが生成されていなかった。

```typescript
declare const symbol1: symbol;

export class A {
  [symbol1]() { return 1; }
}
```

```typescript
// < v5.7での解釈
export class A {}

// v5.7+での解釈
export class A {
  [x: symbol]: () => number;
}
```

これにより、オブジェクトリテラルのメンバ変数と一貫した挙動を提供できるようになった。

## More Implicit any Errors on Functions Returning null and undefined

関数がnullまたはundefinedを返却する場合に `--NoImplicitAny` オプション環境下でエラーが出力されるようになった。

```typescript
declare var p: Promise<number>;

p.catch(() => null);
// error TS7011: Function expression, which lacks return-type annotation, implicitly has an 'any' return type.
```
