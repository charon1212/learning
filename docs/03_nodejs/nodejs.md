---
layout: default
title: nodejs
nav_order: 3
parent: Home
---

# nodejs

## CLI ツール

以下のコミットを元に、過去やった対応をメモ。

- commit: <https://github.com/charon1212/my-project-starter/commit/31be0b6e7bef9e86a7963ebdeb5b5f78edd362ee>
- 手順
  - `npm init -y`で、package.json を作成。
  - `tsc --init`で、tsconfig.json を作成。
  - `tsconfig.json`で outdir プロパティを修正し、build 配下に保存する設定を追加。
  - `package.json`の bin プロパティを追加。
    - `"bin": {"【CLIコマンド名】": "build/index.js"}`
  - `package.json`の scripts に build を追加。
    - `"scripts": {"build": "tsc"}`
  - index.ts を作成。1 行目に`#!/usr/bin/env node`を記載して、CLI であることを明示。
  - `npm run build`でTypeScriptファイルをJavaScriptに変換。
  - `npm link`コマンドでローカル環境にインストール。
- 参考の Qiita 記事: <https://qiita.com/toshi-toma/items/ea76b8894e7771d47e10>
