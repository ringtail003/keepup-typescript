# Checks for Never-Initialized Variables

### TL;DR <a href="#checks-for-never-initialized-variables" id="checks-for-never-initialized-variables"></a>

.初期化されていない変数を参照した時に（内包する関数内でも）エラーが得られるようになった。

```typescript
let foo;

if (condition) {
  foo = 1;
} else {
}

// 初期化していない変数を参照、エラーが得られる
// ERROR: Variable 'result' is used before being assigned.
console.log(foo);
```



```typescript
let foo;

if (condition) {
  foo = 1;
} else {
}

bar();

function bar() {
  // < TS 5.7 エラーにならない
  // TS 5.7+ エラーが得られる
  console.log(foo);
}
```
