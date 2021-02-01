---
title: "Vueで作成したSPAをHerokuでデプロイするまで"
emoji: "📡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vuejs", "heroku", "wsl2"]
published: false
---

# はじめに

Vue で作成した SPA をデプロイする際には [Netlify](https://www.netlify.com/) や Github Pages といったホスティングサービスを使う方法があります。その中で今回は [Heroku](https://jp.heroku.com/) にてデプロイする方法を調べてみました。
以前、[こちら](https://zenn.dev/ryuu/articles/try-vuetifyapp)の記事で VueCLI を使って開発した SPA を実際にデプロイしていきます。

以前 Heroku にアプリをデプロイした経験はありましたが、VueSPA をデプロイした経験がなかったため、今回は Heroku にデプロイし動かせるところまでの流れについてまとめてみます。

# 環境

- Windows10 バージョン 20H2
- WSL2 (Ubuntu 20.04.1 LTS (Focal Fossa))
- Node.js v12.18.4
- Vue CLI 4.5.10
- heroku CLI 7.47.11

# 現状

- 以前 Vagrant を使って Heoku に PHP 製アプリをデプロイした経験はあり
- WSL2 で Heroku を使ったことはない
- Vue CLI で作った SPA を Heroku にデプロイできるのかわからない

# インストール

## Heroku CLI を WSL2 にインストール

Heroku は各プラットフォーム向けに `Heroku CLI` というツールを提供しているためこちらを利用します。
Heroku 公式ドキュメントに各プラットフォームごとの CLI をインストールする方法が列挙されているのでこちらを参考に進めます。
https://devcenter.heroku.com/articles/heroku-cli?source=post_page#download-and-install

ドキュメントに従い以下のコマンドをターミナルで実行。

```shell
$ sudo snap install --classic heroku
```

以下のような**エラー**が出ました。

```shell
error: cannot communicate with server: Post http://localhost/v2/snaps/heroku: dial unix /run/snapd.socket: connect: no such file or directory
```

どうやら WSL2 では `snapコマンド` がうまく動作しない模様です。
そこで以下のコマンドでスタンドアロンインストールを実行します。

```shell
$ sudo curl https://cli-assets.heroku.com/install.sh | sh
```

## バージョン確認

インストールに成功したかを確認するために以下のコマンドで CLI のバージョンを表示してみます。

```shell
$ heroku --version heroku/7.47.11 linux-x64 node-v12.16.2
```

インストールに成功し、WSL2 上にて Heroku CLI が使えるようになりました。

# デプロイする下準備

それでは Heroku CLI を実際に動かしてデプロイする準備をしていきます。

## Heroku へログイン

以下のページから Heroku アカウントを登録します。
https://signup.heroku.com/

アカウントの登録ができたらターミナルからログインをしていきます。

```shell
$ heroku login
  heroku: Press any key to open up the browser to login or q to exit:
  # ブラウザにてログイン処理
```

`q`以外のキーを押すと自動的にデフォルトのブラウザで Heroku のログイン画面が開かれるのでブラウザ上でアカウントへログインします。

以下の画面が表示されたら正常にログインできています。
![Logged In](https://storage.googleapis.com/zenn-user-upload/mwtpkmsj2zy19usm5621nxm5zxxy)

```shell
Logging in... done
  Logged in as '自分のメールアドレス'
```

ターミナルでもログインが成功していることを確認できます。

## 必要なファイルの生成

以下のページに Vue CLI で構築したアプリを Heroku へデプロイする方法が載っていたため参考にして進めていきます。
https://cli.vuejs.org/guide/deployment.html#heroku

デプロイするのに `static.json` というファイルが必要みたいなので、Vue プロジェクトのルートディレクトリに移動して `static.json` を作成します。

```shell
$ touch static.json
```

作成したファイルを以下のように編集して保存します。

```json:project/static.json
{
  "root": "dist",
  "clean_urls": true,
  "routes": {
    "/**": "index.html"
  }
}
```

`static.json` は Heroku へのデプロイで必要になるため Git へ追加しておきます。

先程作成した `static.json` を追加。

```shell
git add static.json
```

コミットする。

```shell
git commit -m "add static configuration"
```
