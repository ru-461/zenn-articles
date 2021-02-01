---
title: "Vueで作成したSPAをHerokuへデプロイし動かすまで"
emoji: "📡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vuejs", "heroku", "wsl2", "個人開発"]
published: true
---

# はじめに

Vue で作成した SPA をデプロイする際には [Netlify](https://www.netlify.com/) や [Github Pages](https://docs.github.com/ja/github/working-with-github-pages/about-github-pages#) といったホスティングサービスを使う方法があります。

その中で今回は [Heroku](https://jp.heroku.com/) にてデプロイする方法を調べてみました。
以前、[こちら](https://zenn.dev/ryuu/articles/try-vuetifyapp)の記事で VueCLI を使って開発した SPA を実際にデプロイしていきます。

以前 Heroku にアプリをデプロイした経験はありましたが、Vue.js で作成した SPA をデプロイした経験がなかったため、今回は Heroku にデプロイし動かせるところまでについてまとめてみます。

# 環境

- Windows10 バージョン 20H2
- WSL2 (Ubuntu 20.04.1 LTS (Focal Fossa))
- Node.js v12.18.4
- Vue CLI 4.5.10
- heroku CLI 7.47.11
- Git 2.25.1

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

ここからは作成したアプリで Git コマンドが使える環境を前提に進めます。
Git のインストール、`git init` でプロジェクトの初期化が済んでいる状態で進めてください。

先程作成した `static.json` をステージング。

```shell
$ git add static.json
```

コミットする。

```shell
$ git commit -m "add static configuration"
```

## Heroku アプリを作成

ターミナルから Heroku へログインしデプロイ対象のプロジェクトのディレクトリで以下のコマンドを実行します。

```shell
$ heroku create
Creating app... done
```

コマンド１つで Heroku プロジェクトが生成されます。
以下のコマンドでアプリのページを開くことができます。

```shell
$ heroku open
```

デフォルトのブラウザが起動してアプリのページが表示されれば正常にアプリが作成されています。
![created app](https://storage.googleapis.com/zenn-user-upload/k8tl4pciubo755hftsnt78k3czvi)
Web ブラウザからアクセスできることが確認できれば OK です。

## アプリをビルドする環境を作成

Vue で作成した SPA はビルドする必要があります。
Heroku 上でアプリをビルドするために以下のビルドパックが必要になるため、それぞれ追加します。

https://elements.heroku.com/buildpacks/heroku/heroku-buildpack-nodejs

```shell
$ heroku buildpacks:add heroku/nodejs
```

https://github.com/heroku/heroku-buildpack-static

```shell
$ heroku buildpacks:add https://github.com/heroku/heroku-buildpack-static
```

上の２つのビルドパックを導入することでアプリをビルドできる環境が出来上がりました。
:::message
ビルドパックは `heroku/node.js` ▶ `heroku-buildpack-static` の順番に追加してください。追加する順番を間違うと上手く動作しない可能性があります。
:::

## Heroku へデプロイする

Heroku へデプロイしていきます。

以下の Git コマンドを実行します。

```shell
$ git push heroku main
```

すぐに Heroku へデプロイ処理が行われます。
このときにビルドも同時に行われ最新の `distディレクトリ` がデプロイされます。

:::message
ドキュメントでは `masterブランチ` にプッシュしていますが、現在は `mainブランチ` に変更されているため注意してください
:::

# デプロイできたかの確認

ブラウザからアプリのページにアクセスして確認してみます。

以下のコマンドを実行することでデフォルトのブラウザでアプリを確認できます。

```shell
$ heroku open
```

![deployed app](https://storage.googleapis.com/zenn-user-upload/p53cv0lij6jtdhcts1c2f09bgv5l)

VueCLI で作成したプロジェクトがブラウザに表示され動作が確認できました。

# さいごに

今回 Heroku を使って Vue で作成した SPA のデプロイ手順をまとめました。
今回使用したのが簡単な SPA だったためすぐにデプロイできました。インターネット上で見る記事では Express を使用してデプロイしている例が多くありましたが、ビルドパックを使うことで簡単にビルド、静的ホスティングまで行ってくれるのに驚きました。

今では優良なホスティングサービスが多く存在するため、Heroku 以外の選択肢もたくさんあります。今回久しぶりにデプロイを行う中でホスティングサービスの充実によってデプロイのハードルが下がってきていると感じました。

お役に立てれば幸いです。最後まで読んでいただきありがとうございました。

# 補足: デプロイ後にアプリ名を変更したい

デフォルトだとランダムでアプリの名前が付与されます。
変更したい場合は CLI を使い以下の手順で変更可能です。

```shell
$ heroku apps:rename 変更したい名前
```

アプリの名前が変わるとアドレスも変わります。
新しいアプリページは名前の変更後にダッシュボード or `heroku open` で開くことができます。
アプリのダッシュボードから変更すると Git remote へ変更がうまく反映されないため、上記のコマンドを使用し、Heroku CLI から変更するのが無難です。

# 参考ドキュメント

- [The Heroku CLI | Heroku Dev Center](https://devcenter.heroku.com/articles/heroku-cli?source=post_page#download-and-install)
- [Deployment | Vue CLI](https://cli.vuejs.org/guide/deployment.html#heroku)
- [WSl 環境における Heroku CLI の導入 - Qiita](https://qiita.com/RoaaaA/items/604f7538d9ef57d2f9c7)
- [Vue.js のアプリケーションを手早く Heroku で公開する - Qiita](https://qiita.com/akirakudo/items/f63322f6851feef3d9e4)
