# The --stableTypeOrdering Flag

## Overview

TypeScript v7 移行をサポートするフラグ。

TypeScript ではコンパイル時に型 ID が割り当てられる。

```typescript
export function foo(condition: boolean) {
    return condition ? 100 : 500;
}
```

v6 では出現順に ID が決定するが、v7 では並列処理されるため出現順にならないことがある。

```typescript
// v6
export declare function foo(condition: boolean): 100 | 500;

// v7
export declare function foo(condition: boolean): 500 | 100;
```

`--stableTypeOrdering` を用いると v7 と同じ結果が得られる。
