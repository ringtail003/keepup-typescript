# Control Flow Analysis for Destructured Discriminated Unions

## TL;DR

タグ付きユニオンを分割代入した時、型が維持されるようになった。

## 前提

タグ付きユニオンとは「Discriminated Unions」「Tagged Union」「直和型」など呼ばれるもの。\
**タグ** によってユニオンを判別する。

{% code title="Unionの例" overflow="wrap" %}
```typescript
type MyUnion = string | number;
```
{% endcode %}

{% code title="タグ付きUnion" overflow="wrap" %}
```typescript
type Response =
  | { status: 200; data: string; }
  | { status: 400; data: Error }
;

if (response.statusCode === 200) {
  response.data.match(/abcde/);
}
if (response.statusCode === 400) {
  response.data.message;
}
```
{% endcode %}

## Previously

分割代入を組み合わせると、ユニオンが判別できなくなっていた。

```typescript
const { statusCode, data } = response;

if (statusCode === 200) {
  data.match(/abc/); // ❌ data ==> string | Error
}
if (statusCode === 400) {
  data.message; // ❌ data ==> string | Error
}
```

## v4.6+

ユニオンの判別ができる。

```typescript
const { statusCode, data } = response;

if (statusCode === 200) {
  data.match(/abc/); // ✅ data ==> string
}
if (statusCode === 400) {
  data.message; // ✅ data ==> Error
}
```
