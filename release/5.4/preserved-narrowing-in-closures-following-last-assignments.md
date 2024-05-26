# Preserved Narrowing in Closures Following Last Assignments

## TL;DR

Nallowingされた型がクロージャ内の処理にも伝搬するようになった。

## Example

以下の例で、クロージャ内の `url` はURL型と認識される。

```typescript
function getUrls(url: string | URL, names: string[]) {
    if (typeof url === "string") {
        url = new URL(url);
    }
    return names.map(name => {
        // url: URL
        url.searchParams.set("name", name);
    });
}
```

ただしクロージャで変更された値が別のクロージャで参照される場合、Nallowingは伝搬されない。

```typescript
function printValueLater(value: string | undefined) {
    // value: string
    if (value === undefined) {
        value = "missing!";
    }
    
    // クロージャ1 value: string
    setTimeout(() => {
        value = value;
    }, 500);
    
    // クロージャ2 value: string | undefined
    setTimeout(() => {
        console.log(value.toUpperCase()); // ERROR
    }, 1000);
}
```
