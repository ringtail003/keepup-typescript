# All \`enum\` s Are Union \`enum\` s

## TL;DR

Enumが共用体として扱われることで、許容しないリテラルをエラーで検出できるようになった。

## サンプル

```typescript
enum Color {
    Red,
    Blue,
    Yellow,
}

function fn(c: Color) {}
fn(Color.Red);
fn(123); // ERROR
```
