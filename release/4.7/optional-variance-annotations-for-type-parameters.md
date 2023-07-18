# Optional Variance Annotations for Type Parameters

## TL;DR

変性に関して型の注釈をin outで指定できるようになった。

## 書き方

`in` または `out` を使って変性を注釈する。\
開発者が型を読み解くヒントとなるものであって、in/outの指定で挙動が変わるものではない。

```typescript
type Provider<out T> = () => T;
type Consumer<in T> = (x: T) => void;
type Mapper<in T, out U> = (x: T) => U;
type Processor<in out T> = (x: T) => T;
```

`in` はT型のパラメータを持つことを注釈する。

```typescript
type Consumer<in T> = (x: T) => void;
```

`out` はT型をリターンすることを注釈する。

```typescript
type Provider<out T> = () => T;
```

注釈と型宣言が一致しない場合、エラーが得られる。

```typescript
type Provider2<out T> = (x: T) => T; // ❌ ERROR
type Consumer2<in T> = (x: T) => T; // ❌ ERROR
```

## 共変と反変

`out` で注釈できる「T型のリターン」は共変の性質を持つ。

```typescript
type Provider<out T> = () => T;

const provide:Provider<B> = () => new B();
const p1:A = provide(); // Aに対してB（サブタイプ）を代入する
const p2:B = provide(); // Bに対してB（型そのもの）を代入する
const p3:C = provide(); // Cに対してB（スーパータイプ）を代入する ❌ ERROR
```

`in` で注釈できる「T型のパラメータ」は反変の性質を持つ。

```typescript
type Consumer<in T> = (x: T) => void;

const consume:Consumer<B> = (x:B) => void 0;
consume(new A()); // AをB（サブタイプ）として使用する ❌ ERROR
consume(new B()); // BをB（型そのもの）として使用する
consume(new C()); // CをB（スーパータイプ）として使用する
```

これらは言語仕様なので `out` `in` で補足しなくても、その性質は変わらない。
