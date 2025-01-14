# Faster Project Ownership Checks in Editors for Composite Projects

## TL;DR

.モノレポプロジェクトでJSファイルを開いた時、tsconfig.jsonの探索が改善された。

```typescript
packages
├── graphics/
│   ├── tsconfig.json
│   └── src/
│       └── ...
├── sound/
│   ├── tsconfig.json
│   └── src/
│       └── ...
├── networking/
│   ├── tsconfig.json
│   └── src/
│       └── ...
├── input/
│   ├── tsconfig.json
│   └── src/
│       └── ...
└── app/
    ├── tsconfig.json
    ├── some-script.js
    └── src/
        └── ...
```

```typescript
// app/tsconfig.json
{
    "compilerOptions": {
        // ...
    },
    "include": ["src"],
    "references": [
        { "path": "../graphics/tsconfig.json" },
        { "path": "../sound/tsconfig.json" },
        { "path": "../networking/tsconfig.json" },
        { "path": "../input/tsconfig.json" }
    ]
}
```



tsonfig.jsonのreferencesを用いると、それぞれのパッケージを順番にビルドできる。\
パッケージAが完了したらパッケージB、完了したらパッケージC...というように、依存などによるビルドの順番を構成する。

なんらかのパッケージに依存するパッケージにはTS内部でcompositeフラグが立てられ、エディタでファイルを開いた時に双方のファイル群が読み込まれる。JSファイルを開いた時、そのファイルが属するtsconfig.jsonおよび依存プロジェクトについての探索が改善された。

以前のバージョンでは、tsconfig.jsonでincludeされていないJSファイルについてモノレポ内のすべてのtsconfig.jsonが探索されていた？公式ドキュメントを把握しきれていない。
