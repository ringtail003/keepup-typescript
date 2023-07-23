# Improved Inference for infer Types in Template String Types

## TL;DR

inferがテンプレートリテラルに展開できるようになった。

## 例

extendsに続けてプリミティブを書くと、リテラルとして展開される。

```typescript
type A<T> = T extends `¥${infer U extends number}` ? `${U}` : T;

type A1 = A<"¥100">; // "100"
type A2 = A<"-">; // "-"
```

ただし"1.0"のようにプリミティブに展開した値と（1になる）元の値（1.0）が違う場合はリテラルとみなされず、期待する型が得られない。
