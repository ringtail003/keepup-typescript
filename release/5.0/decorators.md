# Decorators

## TL;DR

Decorators構文がサポートされた。

## Decoratos

[https://github.com/tc39/proposal-decorators](https://github.com/tc39/proposal-decorators)

2024/02時点でStage3。\
Chromeをはじめとする環境で全て未サポート。

以前のバージョンはデコレーター構文がエラーになっていたが、v5.0から構文はエラーにならずTSコンパイラを通してJSコードに変換されるようになった。

```typescript
"use strict";
var __runInitializers = (this && this.__runInitializers) || function (thisArg, initializers, value) {
    var useValue = arguments.length > 2;
    for (var i = 0; i < initializers.length; i++) {
        value = useValue ? initializers[i].call(thisArg, value) : initializers[i].call(thisArg);
    }
    return useValue ? value : void 0;
};
var __esDecorate = (this && this.__esDecorate) || function (ctor, descriptorIn, decorators, contextIn, initializers, extraInitializers) {
    function accept(f) { if (f !== void 0 && typeof f !== "function") throw new TypeError("Function expected"); return f; }
    var kind = contextIn.kind, key = kind === "getter" ? "get" : kind === "setter" ? "set" : "value";
    var target = !descriptorIn && ctor ? contextIn["static"] ? ctor : ctor.prototype : null;
    var descriptor = descriptorIn || (target ? Object.getOwnPropertyDescriptor(target, contextIn.name) : {});
    var _, done = false;
```

デコレーターは規定のシグネチャを持つ。

```typescript
function foo(originalMethod: any, context: ClassMethodDecoratorContext) {}
```

引数contextは以下のインターフェースを持つ。

```typescript
type Decorator = (value: Input, context: {
  kind: string;
  name: string | symbol;
  access: {
    get?(): unknown;
    set?(value: unknown): void;
  };
  private?: boolean;
  static?: boolean;
  addInitializer?(initializer: () => void): void;
}) => Output | void;

```

Classのメソッドに対するデコーレーター。

```typescript
class Person {
    @foo
    greet() {}
}
```

Classのアクセサに対するデコレーター。

```typescript
class Person {
    @foo accessor clicked = false;
}
```

ドキュメントを見ると、以下4種類に設置可能。

* Classes
* Class fields (public, private, and static)
* Class methods (public, private, and static)
* Class accessors (public, private, and static)
