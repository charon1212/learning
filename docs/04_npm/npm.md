---
layout: default
title: npm
nav_order: 1
---

# npm

## NPMリポジトリ自作

NPMリポジトリの初期化手順を書いておく。

- gitリポジトリを作ってclone
- `npm init -y`と`tsc --init`を実行
- package.json編集：
  - nameの手前に`@<npm-name>/`を追加。
  - 以下を追加。

```json
{
  "main": "build/index.js",
  "types": "build/index.d.ts",
  "scripts": {
    "prepublishOnly": "tsc",
    "build": "tsc"
  },
  "files": [
    "build",
    "README.md",
    "LICENSE",
    "package.json"
  ],
  "license": "MIT",
}
```

- tsconfig.jsonを編集：

```json
{
  "declaration": true,
  "outDir": "./build",
}
```

- src/index.tsを追加。
- CHANGELOG.mdを追加。

```md
# Change Log

## [0.0.0] - yyyy-MM-dd

- テストプッシュ

## [1.0.0] - yyyy-MM-dd

- 新規追加。
```

- 以下を実行：`npm publish --access public`
