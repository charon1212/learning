---
layout: default
title: electron_react
nav_order: 7
parent: Home
---

# Electron-React

よく使う Electron + React の組み合わせについてメモ的に残しておく。

## 環境構築

基本はElectronForgeを使ってTypeScript+Electronのテンプレートを作成する。続いて、React関連のモジュールを追加して、rendererに組み込む。

- ElectronForgeのテンプレート
  - コマンドを実行してテンプレートを導入(これだけでnpm run start 実行可)
    - `$ npx create-electron-app . --template=vite-typescript`
- React追加
  - コマンドでReact関連コンポーネントを追加
    - `$ npm i react react-dom`
    - `$ npm i -D @types/react @types/react-dom`
  - `tsconfig.json`を編集 (*1)
  - `src/renderer/App.tsx`を作成(※中身は何でもOK)
  - `renderer.ts`を`renderer.tsx`にリネーム
  - `renderer.tsx`を編集し、Appを読み込むよう設定(*2)
  - `index.html`を編集し、スクリプトを`renderer.ts`から`renderer.tsx`に変更し、`id="root"`のdiv要素を追加(*3)

(*1): tsconfig.json

```json
{
  "compilerOptions": {
    "jsx": "react-jsx" // ここをcompilerOptionsに追加
  }
}
```

(*2): renderer.tsx

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { App } from './renderer/App';

const container = document.getElementById('root');

if (container) {
  const root = ReactDOM.createRoot(container);
  root.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>,
  );
} else {
  console.error('#root element not found');
```

(*3): index.html

```html
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello World!</title>

  </head>
  <body>
    <div id="root"></div> <!-- ここがReact要素に置き換わる -->
    <script type="module" src="/src/renderer.tsx"></script> <!-- 既存はrenderer.tsなので、tsxにする -->
  </body>
</html>
```
