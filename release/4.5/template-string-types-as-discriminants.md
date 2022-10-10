# Template String Types as Discriminants

ユニオン型の判別にテンプレートリテラルが使えるようになった。

## おさらい

### ユニオン型（Union Type）

`|` を使った型のOR条件。

```typescript
type A = {};
type B = {};
type MyUnion = A | B; // <=== AまたはB
```

```typescript
function doSomething(
  value: number | string // <=== numberまたはstring
) { ... }
```

### テンプレートリテラル（Template Literal Types）

`` `${}` ``を使った文字列テンプレート。

```typescript
type Lang = "en" | "ja" | "pt";
type Locale = `i18n-${Lang}`; // <=== これ

const locale: Locale = "i18n-ja";
```

## 本題

例として、システムエラーの種類をユニオン型で宣言する。

```typescript
// 致命的エラー: XXXCriticalError
type CriticalError = {
  id: `${string}CriticalError`;
  redirectUrl: URL;
};

// 一時的なエラー: XXXTempolaryError
type TemporaryError = {
  id: `${string}TempolaryError`;
  displayMessage: string;
};

// ユニオン型で束ねておく
type SystemError = CriticalError | TemporaryError;
```

ユニオン型の「どの型にマッチするのか」をテンプレート文字列で指定できるようになった。

```typescript
function errorHandler(error: SystemError) {
  if (error.id === "DBConnectionCriticalError") {
    location.href = error.redirectUrl.href;
  }

  if (error.id === "MaintenanceTempolaryError") {
    window.alert(error.displayMessage);
  }
  ...
}
```

ifの条件分岐だけでなくswitchでも使える。

```typescript
function errorHandler(error: SystemError) {
  switch(error.id) {
    case "DBConnectionCriticalError": 
      location.href = error.redirectUrl.href;
      return;
    case "MaintenanceTempolaryError": 
      window.alert(error.displayMessage);
      return;
    ...
  }
}
```

間違ったプロパティの参照はコンパイルエラーで気付くことができる。

```typescript
// CriticalErrorはdisplayMessageを持っていない
if (error.id === "DBConnectionCriticalError") {
  error.displayMessage; // Error: Property does not exist on type
}
```
