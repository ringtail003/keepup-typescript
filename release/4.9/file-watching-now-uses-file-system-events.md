# File-Watching Now Uses File System Events

## TL;DR

ファイル監視のパフォーマンスが向上した。



[https://www.typescriptlang.org/docs/handbook/configuring-watch.html](https://www.typescriptlang.org/docs/handbook/configuring-watch.html)

```typescript
{
  "compilerOptions": {
    "target": "es2020",
    "moduleResolution": "node"
    // ...
  },
  "watchOptions": {
    // Use native file system events for files and directories
    "watchFile": "useFsEvents",
    "watchDirectory": "useFsEvents",
```

tsconfig.jsonに `watchOptions` というスキーマがあり、ファイル監視に何を使用するかカスタムできる。v3.8に登場したが、v4.9でファイル変更イベントドリブンの挙動がデフォルトになったため、ローカルセットアップで使用する必要はなくなった。

コードがリモートにありSSHなどでネットワーク越しに監視する場合、チューニングとして使う。
