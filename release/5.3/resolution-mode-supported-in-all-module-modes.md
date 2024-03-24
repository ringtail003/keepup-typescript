# \`resolution-mode\` Supported in All Module Modes

## TL;DR

`resolution-mode` の指定がすべての `moduleResolution` オプションで使えるようになった。

```typescript
// tsconfig.json
{
    moduleResolution: "node16"
}
```

以前は `node16` または `nodenext` で `resolution-mode` を使うことができた。\
v5.3から `bundler` `node10` `classic` でも使えるようになった。
