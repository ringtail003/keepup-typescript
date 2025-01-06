# Iterator Helper Methods

## TL;DR

ESのIterator Helperに対応するため、イテレータの新しい型が導入された。

[https://github.com/tc39/proposal-iterator-helpers](https://github.com/tc39/proposal-iterator-helpers) により、TS従来のIterator型と、JSランタイムのIterator型が衝突するようになった。これを解消するためv5.6でIteratorObjectが導入された。

### JSランタイムのIterator Helperが使えるようになった

```typescript

function* positiveIntegers() {
    let i = 1;
    while (true) {
        yield i;
        i++;
    }
}

// take, filter, mapなど、Arrayに存在するメソッドがIteratorでも使える
const evenNumbers = positiveIntegers().map(x => x * 2);

for (const value of evenNumbers.take(5)) { ... }
```

### イテレータの型が柔軟に表現できるようになった

{% code overflow="wrap" %}
```typescript
interface IteratorObject<T, TReturn = unknown, TNext = unknown> extends Iterator<T, TReturn, TNext> {
    [Symbol.iterator](): IteratorObject<T, TReturn, TNext>;
}

// T: コレクションの要素の型
// TReturn: イテレータが終了する時の要素の型
// TNext: next()の引数の型


// シンプルにnumberを返し続けるイテレータ
const iterator: IteratorObject<number>;

// 最後にstringを返すイテレータ
const iterator: IteratorObject<number, string>;

// nextに渡したbooleanによって返却されるnumberの値が変わるイテレータ
const iterator: IteratorObject<number, void, boolean>;
```
{% endcode %}

