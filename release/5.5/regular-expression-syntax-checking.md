# Regular Expression Syntax Checking

## TL;DR

正規表現のシンタックスチェックが導入された。\
これまでTSでは正規表現に関してはシンタックスチェックがなく、そのままJSにトランスパイルしていた。

## Example

```typescript
// かっこが一致しない
let myRegex = /@robot(\s+(please|immediately)))? do some task/;

// ERROR: Unexpected ')'. Did you mean to escape it with backslash?
```

{% code overflow="wrap" %}
```typescript
// 存在しない後方参照
let myRegex = /@typedef \{import\((?<importPath>.+)\)\.(?<importedEntity>[a-zA-Z_]+)\} \k<namedImport>/;

// ERROR: There is no capturing group named 'namedImport' in this regular expression.
```
{% endcode %}

{% code overflow="wrap" %}
```typescript
// tsconfig.jsonのtargetで指定されたECMAScriptにない機能
let myRegex = /@typedef \{import\((?<importPath>.+)\)\.(?<importedEntity>[a-zA-Z_]+)\} \k<importedEntity>/;

// ERROR: Named capturing groups are only available when targeting 'ES2018' or later.
```
{% endcode %}
