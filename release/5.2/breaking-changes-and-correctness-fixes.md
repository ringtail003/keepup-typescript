# Breaking Changes and Correctness Fixes

## lib.d.ts Changes

`TupleType.labeledElementDeclarations` がundefinedを含むようになった。

## \`module\` and \`moduleResolution\` Must Match Under Recent Node.js settings

`--module` と `--moduleResolution` の整合性がチェックされるようになった。

{% code overflow="wrap" %}
```typescript
{
    module: "esnext",
    moduleResolution: "node16",
}
// ERROR: Option 'moduleResolution' must be set to 'NodeNext' (or left unspecified) when option 'module' is set to 'NodeNext'.
```
{% endcode %}

{% code overflow="wrap" %}
```typescript
{
    module: "esnext",
    moduleResolution: "bundler",
}
// ERROR: Option 'module' must be set to 'Node16' when option 'moduleResolution' is set to 'Node16'.
```
{% endcode %}

## Consistent Export Checking for Merged Symbols

以下のようなケースで `replaceInFile` がマージされることに警告が出るようになった。

{% code overflow="wrap" %}
```typescript
declare module 'replace-in-file' {
    export function replaceInFile(config: unknown): Promise<unknown[]>;
    export {};
    namespace replaceInFile {
        export function sync(config: unknown): unknown[];
  }
}

// Individual declarations in merged declaration 'replaceInFile' must be all exported or all local.
```
{% endcode %}
