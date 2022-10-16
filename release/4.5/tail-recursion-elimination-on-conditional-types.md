# Tail-Recursion Elimination on Conditional Types

Conditional Typesの末尾再帰の削除が実装された。

## TL;DR

以前のバージョンでは型の再帰が深いとエラーになっていた。

```typescript
type TrimLeft<T extends string> =
    T extends ` ${infer Rest}`
      ? TrimLeft<Rest> // 👈 再帰
      : T;
    
// Error: Type instantiation is excessively deep and possibly infinite.
type Test = 
  TrimLeft<"                                                無駄無駄無駄無駄">;
```

TS4.5で末尾再帰の最適化が導入されエラーは出なくなった。

```typescript
type TrimLeft<T extends string> =
    T extends ` ${infer Rest}`
      ? TrimLeft<Rest>
      : T;
    
// OK: "無駄無駄無駄無駄"
type Test = 
  TrimLeft<"                                                無駄無駄無駄無駄">;
```

ただし結果をUnion Typeにすると最適化が適用されませんよ、という話。

{% code overflow="wrap" %}
```typescript
type GetChars<S> =
    S extends `${infer Char}${infer Rest}`
        ? Char | GetChars<Rest> // 👈 Union Type
        : never;

// ネストが浅ければOK
// OK: "無" | "駄"
type Chars = GetChars<"無駄無駄無駄無駄無駄">

// ネストが深すぎるとNG
// Error: Type instantiation is excessively deep and possibly infinite.
type Chars = GetChars<"無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄無駄">;
```
{% endcode %}

## 再帰

プログラミングの一般的な手法で自分自身を呼び出す「再帰」というものがある。

```typescript
function fn(n) {
  if (n === 0) {
    return n;
  }
  return fn(n - 1); // 👈 再帰
}
```

ユースケースは実行時でないとネストの深さが分からないもの。ツリー上になったDOM構造や、中に何が入っているのか分からないPromiseやZipファイルなんかにも当てはめて考えられる。

```typescript
// zipファイルを解凍するイメージ
function unzip(file) {
  if (file.type !== "zip") {
    return file;
  }
  return unzip(file.getFile());
}
```

関数の一番最後で再帰することを「末尾再帰」と呼ぶ。

```typescript
// 末尾再帰
function fn(n) {
  ...
  return fn(n - 1); // 👈 再帰
}

// 末尾再帰ではない
function fn(n) {
  ...
  fn(n - 1); // 👈 再帰
  return 1;
}
```

## 再帰とオーバーフロー

末尾再帰はJavaScriptでどんなふうに演算が行われているか？雑な説明で考えよう。

```typescript
function fn(n, char) {
  if (n === 0) return char;
  return fn(n - 1, `${char} world`);
}

fn(3, "the"); // "the world world world"
```

関数は1回呼ばれ、3回再帰している。

```typescript
fn(3, "the");
fn(2, "the world");
fn(1, "the world world");
fn(0, "the world world world");
```

関数が呼ばれるとコールスタックにスタックフレームが積まれていく。

```typescript
4回目 | fn{n:0,char:"the world world world"}
3回目 | fn{n:1,char:"the world world"}
2回目 | fn{n:2,char:"the world"}
1回目 | fn{n:3,char:"the"}
```

スタックフレームがある一定数を超えると、メモリを食いつぶさないようにオーバーフローが発生する。

<pre class="language-typescript"><code class="lang-typescript"><strong>n回目 | RangeError: Maximum call stack size exceeded
</strong><strong>...
</strong><strong>4回目 | fn{n:0,char:"the world world world"}
</strong>3回目 | fn{n:1,char:"the world world"}
2回目 | fn{n:2,char:"the world"}
1回目 | fn{n:3,char:"the"}</code></pre>

この関数の例では6070回あたりでオーバーフローした（Chrome v108）。

## 再帰の最適化

オーバーフローを避けるため、forやwhileなどに書き換える解決策がある。

```typescript
function fn() {
  let n = 0;
  let char = "the";
  
  while (n < 10000) {
    char = `${char} world`;
    n++;
  }
}

fn();
```

forやwhileは1つしかスタックフレームが積まれないので、スタックフレームのオーバーフローは発生しない。

```typescript
1回目 | fn{n:3,char:"the"}
```

```typescript
2回目 | fn{n:2,char:"the world"}
```

```typescript
3回目 | fn{n:1,char:"the world world"}
```

このような最適化をサポートする言語は、自分で書き換えなくともコンパイラがfor/while相当の処理に変換してくれる。JavaScriptでも言語レベルのサポートが検討されES6に仕様が盛り込まれているが、2022/10時点でSafariにしか実装されていない。残念！

再帰はなんでもかんでも最適化される訳ではなく、関数の一番最後で再帰する「末尾再帰」が対象になる。

```typescript
function fn(n) {
  ...
  return fn(n - 1); // 👈 末尾再帰
}
```

```typescript
// このように末尾再帰が積まれていくところを...
4回目 | fn{n:0}
3回目 | fn{n:1}
2回目 | fn{n:2}
1回目 | fn{n:3}
```

```typescript
// 最適化によって積まないようにする（末尾再帰の削除）
1回目 | fn{n:3}
```

## TypeScriptの最適化

本題に戻る。\
TypeScriptの型システムでは「Conditional Typesの末尾再帰の削除」が実装された。\
これはJavaScriptのコードでなく型における最適化の話。

### 末尾再帰の削除

Conditional Typesとは `T extends A ? B : C` のような型システム上の三項演算子のこと。

```typescript
type TrimLeft<T extends string> =
    T extends ` ${infer Rest}`
      ? TrimLeft<Rest>
      : T;
```

TrimLeftの型をコードで示すとこのようなものになる。

```typescript
function trimLeft(str) {
  return str.charAt(0) === " "
    ? trimLeft(str.substr(1))
    : str;
}

// 空白の個数分、再帰する
trimLeft("     無駄無駄無駄無駄無駄");

// 再帰が多すぎるとオーバーフロー（以前のバージョン）
// RangeError: Maximum call stack size exceeded
trimLeft("                                          無駄無駄無駄無駄無駄");
```

そこまで詳しくないので実際どうなっているのか分からないが、末尾再帰の削除によりfor/whileに該当するような処理が行われているのだろうと思う。

```typescript
function trimLeft(str) {
  let s = str;
  while (s.charAt(0) === " ") {
    s = s.substr(1);
  }
  return s;
}

// オーバーフローしない
trimLeft("                                          無駄無駄無駄無駄無駄");
```

V4.5で導入された「末尾再帰の削除」によって、Conditional Typeのオーバーフローは発生しなくなった。

```typescript
// 以前のバージョン: RangeError
type X = TrimLeft("                                   無駄無駄無駄無駄無駄");

// V4.5: OK
type X = TrimLeft("                                   無駄無駄無駄無駄無駄");
```

### Union Typeは末尾再帰が削除されない

Union Typeとはパイプを使ったOR条件のこと。

```typescript
type A = string | number;
```

OR条件は組み合わせを導出するため、TrimLeftのように1つの型を導出する結果にはならない。

```typescript
type GetChars<S> =
    S extends `${infer Char}${infer Rest}`
        ? Char | GetChars<Rest> // 👈 Union Type
        : never;
```

コードで書き換えると末尾再帰にならないことが分かる。

```typescript
function getChars(str, acc = []) {
  if (str.length === 0) {
    return acc;
  }

  // 再帰の結果を待ってから戻り値が決まる
  return getChars(str.substr(1), [...acc, str.charAt(0)]);
}

// ["1", "2", "3"]
getChars("123")
```

Union Typeでなければ末尾再帰として扱われるため、結果を保存するための中間型を置くことで解決できる。

```typescript
type GetChars<S> = GetCharsHelper<S, never>;

type GetCharsHelper<S, Acc> = 
    S extends `${infer Char}${infer Rest}`
        ? GetCharsHelper<Rest, Char | Acc> // 👈 Union Typeではない
        : Acc;
```
