# extends Constraints on infer Type Variables

## TL;DR

inferにextendsの条件指定が付けられるようになった。

## Prev

inferとextendsは分けて書く必要があった。

```typescript
type FirstIfString<T> = 
    T extends [infer S, ...unknown[]] ?
        S extends string ? S : never 
        : never;

type F1 = FirstIfString<[string, number]>; // string
type F2 = FirstIfString<["hello", number]>; // "hello"
```

## Current

inferに続けてextendsを書けるようになった。

```typescript
type FirstIfString<T> = 
    T extends [infer S extends string, ...unknown[]] ?
        S : never;

type F1 = FirstIfString<[string, number]>; // string
type F2 = FirstIfString<["hello", number]>; // "hello"
```
