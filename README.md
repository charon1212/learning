# learning

学習用のまとめリポジトリ。

docs: <https://charon1212.github.io/learning/>

## フォルダ構成

- code: テスト用のコードなどを配置する場所。
- docs: markdown配置場所。この配下をgithub pagesでデプロイ。

## GitHub Pages

deploy at <https://charon1212.github.io/learning/>

- 最初にやったこと
  - リポジトリページの「Settings」を選択
  - 左欄の「Pages」から、ブランチ(master)/ディレクトリ(/docs)を選択。
    - ※docsディレクトリとroot以外はなぜか選べず…
  - git pushに反応してビルド実施。markdownをビルドしてHTMLにしてデプロイしてくれる。
- スタイルの追加
  - 四苦八苦した結果、以下のことが分かる。
    - GitHub Pagesは`Jekyll`というライブラリを利用しているため、[JekyllのConfigurationマニュアル](https://jekyllrb.com/docs/configuration/)を元に`_config.yaml`を **docs** ディレクトリに配置することで、様々なカスタマイズ可能。（GithubPagesで公開ディレクトリにdocsを指定しているため、docs配下に配置が必要らしい。(要出典)）
      - ※[Githubマニュアル](https://docs.github.com/ja/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll)
    - [JekyllのPagesマニュアル](https://jekyllrb.com/docs/pages/)にあるように、index.mdにしないと`/`でアクセスできないので注意。本リポジトリのトップページは`index.md`とした。
    - [just the docs](https://just-the-docs.com/)というテーマが使いやすそうなので導入。
    - 各mdファイルの先頭には、Front MatterというJekyllの設定を入れている。詳細は、[JekyllのFrontMatterマニュアル](https://jekyllrb.com/docs/front-matter/)を参照。
