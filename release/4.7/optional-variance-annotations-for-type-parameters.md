# Optional Variance Annotations for Type Parameters

## TL;DR

変性に関して型の注釈をin outで指定できるようになった。

## 書き方

`in` または `out` を使って変性を注釈する。\
開発者が型を読み解くヒントとなるものであって、指定によって挙動が変わるものではない。

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
type Provider2<out T> = (x: T) => T; // ❌ ERROR T型をパラメータに取ることが宣言されていない
type Consumer2<in T> = (x: T) => T; // ❌ ERROR T型をリターンすることが宣言されていない
```

## 共変

`out` で注釈した「T型のリターン」は**共変**の性質を持つ。\
型そのもの＋サブタイプを許容する。

```typescript
class A { a() {} }
class B extends A { b() {} }
class C extends B { c() {} }

type Provider<out T> = () => T;
const provide:Provider<B> = () => new B();

const p1:A = provide(); // Aに対してB（サブタイプ）を代入する
const p2:B = provide(); // Bに対してB（型そのもの）を代入する
const p3:C = provide(); // Cに対してB（スーパータイプ）を代入する ❌ ERROR
```

## 反変

`in` で注釈した「T型のパラメータ」は**反変**の性質を持つ。\
型そのもの＋スーパータイプを許容する。

```typescript
class A { a() {} }
class B extends A { b() {} }
class C extends B { c() {} }

type Consumer<in T> = (x: T) => void;
const consume:Consumer<B> = (x:B) => void 0;

consume(new A()); // AをB（サブタイプ）として使用する ❌ ERROR
consume(new B()); // BをB（型そのもの）として使用する
consume(new C()); // CをB（スーパータイプ）として使用する
```

## 不変

`in out` で注釈した「T型のパラメータ」「T型のリターン」は相互作用で**不変**の性質を持つ。\
型そのものを許容する。

```typescript
class A { a() {} }
class B extends A { b() {} }
class C extends B { c() {} }

type Processor<in out T> = (x: T) => T;
const process:Processor<B> = (x:B) => new B();

const pr1:A = process(new A()); // ❌ ERROR（パラメータ）
const pr2:B = process(new B());
const pr3:C = process(new C()); // ❌ ERROR（代入）
```

## 双変

関数パラメータの共変は `strictFunctionChecks:false` にすると**双変**の性質に変わる。\
型そのもの＋サブタイプ＋スーパータイプを許容する。

<pre class="language-typescript"><code class="lang-typescript"><strong>// tsconfig.json
</strong><strong>strictFunctionChecks: true  
</strong>
class A { a() {} }
class B extends A { b() {} }
class C extends B { c() {} }

type TakeB = (arg: B) => void;

const t1: TakeB = (arg: A) => {};
const t2: TakeB = (arg: B) => {};
const t3: TakeB = (arg: C) => {}; // ❌ ERROR
</code></pre>

<pre class="language-typescript"><code class="lang-typescript"><strong>// tsconfig.json
</strong><strong>strictFunctionChecks: false
</strong>
class A { a() {} }
class B extends A { b() {} }
class C extends B { c() {} }

type TakeB = (arg: B) => void;

const t1: TakeB = (arg: A) => {};
const t2: TakeB = (arg: B) => {};
const t3: TakeB = (arg: C) => {};
</code></pre>
