# Optional Variance Annotations for Type Parameters

## TL;DR

変性に関して型の注釈をin outで指定できるようになった。

## Current

`<in out T>` としてジェネリクスを指定する。

```typescript
interface State<in out T> {
    get: () => T;
    set: (value: T) => void;
}
```

WIP
