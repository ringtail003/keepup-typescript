# Control Flow Analysis for Destructured Discriminated Unions

## TL;DR

タグ付きユニオンを分割代入した時、型が維持されるようになった。

## 前提

ユニオンとは、複数の型をOR条件で列挙するもの。

{% code title="Unionの例" overflow="wrap" %}
```typescript
type MyUnion = string | number;
```
{% endcode %}

**タグ** の値を絞り込むことで、その他のプロパティの型が絞り込まれる。\
この方法を「タグ付きユニオン」と呼ぶ。\
別名：Discriminated Unions、Tagged Union、直和型

{% code title="タグ付きUnion" overflow="wrap" %}
```typescript
type Response =
  | { statusCode: 200; data: string; }
  | { statusCode: 400; data: Error }
;

if (response.statusCode === 200) {
  response.data.match(/abcde/);
}
if (response.statusCode === 400) {
  response.data.message;
}
```
{% endcode %}

## Previous

分割代入を組み合わせると、プロパティの型が判別できなかった。

```typescript
const { statusCode, data } = response;

if (statusCode === 200) {
  data.match(/abc/); // ❌ data: string | Error
}
if (statusCode === 400) {
  data.message; // ❌ data: string | Error
}
```

## Current

プロパティの型が判別できるようになった。

```typescript
const { statusCode, data } = response;

if (statusCode === 200) {
  data.match(/abc/); // ✅ data: string
}
if (statusCode === 400) {
  data.message; // ✅ data: Error
}
```
