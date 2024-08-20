# Inferred Type Predicates

## TL;DR

Array.filterで型が絞り込まれるようになった。

```typescript
const values = [1,2,3,null];

values
  .filter(v => v !== null)
  .reduce((a, b) => a + b);

// Previous: a is possibly null.
// Current: 
```

### 絞り込まれるパターン

```typescript
values
  .filter(v => typeof v === "number")
  .reduce((a, b) => a + b);
```

```typescript
values
  .filter(v => v !== null)
  .reduce((a, b) => a + b);
```

```typescript
values
  .filter(v => isNumber(v))
  .reduce((a, b) => a + b);

const isNumber = (v: any): v is number => {
  return typeof v === "number";
};
```

### 絞り込まれないパターン

```typescript
// assertion function
values
  .filter(v => isNumber(v))
  .reduce((a, b) => a + b);

const isNumber = (v: any): asserts v is number => {
  return;
};
```

```typescript
// 戻り値が真偽値ではない、0を除外
values
  .filter(v => !!v)
  .reduce((a, b) => a + b);
```

```typescript
// パラメータの変更
values
  .filter(v => {
    if (v === 2) { v = 20; }
    return v !== null;
  })
  .reduce((a, b) => a + b);
```

```typescript
// 単一の戻り値ではない
values
  .filter(v => {
    if (v === null) { 
      return false;
    }
    return true;
  })
  .reduce((a, b) => a + b);
```
