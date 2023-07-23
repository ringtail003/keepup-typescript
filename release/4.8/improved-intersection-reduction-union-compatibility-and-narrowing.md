# Improved Intersection Reduction, Union Compatibility, and Narrowing

## TL;DR

undefined,nullを除外する推論が改善された。

## undefined,nullを除外するNarrowing

以前のバージョンでは、{}にundefinedとnullが入る余地があった。\
これによりunknown型やジェネリクスなどのNarrowingで期待どおりの結果が得られない事があった。

### unknownを使ったNarrowing

```typescript
function fn(x: unknown) {
    if (x === undefined || x === null) {
        throw new Error("");
    }
    return x;
}

declare const x: unknown;
fn(x);
```

以前のバージョンでは推論結果がunknownとなってしまう。

```typescript
fn(x); // unknown
```

4.8ではundefindとnullは含まれなくなった。

```typescript
fn(x); // {}
```

### ジェネリクスを使ったNarrowing

```typescript
function fn<T>(x: T) {
    if (x === undefined || x === null) {
        throw new Error("");
    }
    return x;
}

declare const x: null | undefined | number;
fn(x);
```

以前のバージョンでは推論結果にundefinedまたはnullが含まれてしまう。

```typescript
fn(x); // null | undefined | number
```

4.8ではundefinedとnullは含まれなくなった。

```typescript
fn(x): // number
```

### ifを使ったNarrowing

```typescript
declare const x: unknown;

if (x) {
    x;
}
```

以前のバージョンではunknownのまま。

```typescript
if (x) {
    x; // unknown
}
```

4.8ではundefinedとnullが除外され、{}と推論される。

```typescript
if (x) {
    x; // {}
}
```

## NonNullable

NonNullableの定義も刷新された。

```typescript
- type NonNullable<T> = T extends null | undefined ? never : T;
+ type NonNullable<T> = T & {};
```

4.8ではunknownからundefinedとnullを除外した型を表現できるようになった。

```typescript
type A = NonNullable<unknown>; // {}
```

以下のコードは、以前のバージョンでは期待通りの結果が得られなかった。

```typescript
function fn<T>(x: T): NonNullable<T> {
  if (x !== null && x !== undefined) {
      return x; // ❌ ERROR: NonNullable<T>と互換性がない
  }
  throw new Error("");
}

declare const x: unknown;
const y = fn(x); // ❌ unknownと推論される
```

4.8では期待どおりの結果が得られるようになった。

```typescript
function fn<T>(x: T): NonNullable<T> {
  if (x !== null && x !== undefined) {
      return x;
  }
  throw new Error("");
}

declare const x: unknown;
const y = fn(x); // ✅ {}と推論される
```
