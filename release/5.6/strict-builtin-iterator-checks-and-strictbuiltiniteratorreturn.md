# Strict Builtin Iterator Checks (and --strictBuiltinIteratorReturn)

オプションを有効にする。

```typescript
// tsconfig.json
strictBuiltinIteratorReturn: true
```

BuiltinIteratorReturnをジェネリクスに与える。\
最後に返却する型がunknownとなり、型安全になるらしい。

```typescript
function* createIterator(): Iterator<string, BuiltinIteratorReturn> {
    yield "a";
    yield "b";
    return 100;
}

const iterator = createIterator();
let result = iterator.next();

if (!result.done) {
    result.value.toUpperCase(); // "A", "B"
} else {
    console.log(result.value); // unknown: 100
}

// BuiltinIteratorReturnを指定しない時：最後に返却する要素はany型
// BuiltinIteratorReturnを指定した時：最後に返却する要素はunknown型
```
