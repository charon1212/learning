---
layout: default
title: clasp
nav_order: 1
parent: google_apps_script
---

# clasp

## 導入手順

1. Node.jsをインストール
    - Node.js公式サイト: <https://nodejs.org/>
    - 推奨されるLTSバージョンをインストールします。
2. claspのインストール
    - ターミナル（またはコマンドプロンプト）で以下のコマンドを実行してclaspをインストールします。

        ```bash
        npm install -g @google/clasp
        ```

    - インストールが成功したか確認します。

        ```bash
        clasp --version
        ```

3. Googleアカウントに認証
    - claspをGoogleアカウントと連携します。

        ```bash
        clasp login
        ```

    - 指示に従い、Googleアカウントで認証を完了します。
4. Apps Scriptプロジェクトのローカル同期
    - Google Apps Scriptのプロジェクトを作成済みの場合は、以下のコマンドでローカルにクローンします。

        ```bash
        clasp clone <SCRIPT_ID>
        ```

        - `<SCRIPT_ID>`はApps ScriptプロジェクトのIDです。
        - 取得方法: Google Apps Scriptエディタで「ファイル」→「プロジェクトの設定」を開き、スクリプトIDを確認します。
    - 新規プロジェクトを作成する場合:

        ```bash
        clasp create --type standalone --title "My Project"
        ```

5. VSCodeで編集
    - プロジェクトをVSCodeで開き、*.gsファイルやappsscript.jsonファイルを編集します。
6. 変更をGoogleにアップロード
    - 編集後、以下のコマンドでGoogleに反映します。

        ```bash
        clasp push
        ```

7. Google側の変更をローカルに取得
    - Google側で直接変更が加えられた場合、以下のコマンドでローカルに同期します。

        ```bash
        clasp pull
        ```
