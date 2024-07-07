---
title: "Vueで作成したSPAをHerokuへデプロイし動かすまで"
emoji: "📡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vuejs", "heroku", "wsl2", "個人開発"]
published: true
---

## はじめに

Vueで作成したSPAをデプロイする際には[Netlify](https://www.netlify.com)や[GitHub Pages](https://docs.github.com/ja/github/working-with-github-pages/about-github-pages#)といったホスティングサービスを使う方法があります。

その中で今回は[Heroku](https://jp.heroku.com)にてデプロイする方法を調べてみました。
以前、[こちら](https://zenn.dev/ryuu/articles/try-vuetifyapp)の記事でVue CLIを使って作成したSPAを実際にHerokuへデプロイしていきます。

以前、Herokuにアプリをデプロイした経験はありましたが、Vue.jsで作成したSPAをデプロイした経験がなかったため、今回はHerokuにデプロイし動かせるところまでについてまとめてみます。

## 環境

- Windows10バージョン20H2
- WSL2（Ubuntu 20.04.1 LTS）
- Node.js v12.18.4
- Vue CLI 4.5.10
- Heroku CLI 7.47.11
- Git 2.25.1

## 現状

- 以前Vagrantを使ってHeokuにPHP製アプリをデプロイした経験はあり
- WSL2でHerokuを使ったことはない
- Vue CLIで作ったSPAをHerokuにデプロイできるのかわからない

## インストール

### Heroku CLIをWSL2にインストール

Herokuは各プラットフォーム向けにHeroku CLIというツールを提供しているためこちらを利用します。
Heroku公式ドキュメントに各プラットフォームごとのCLIをインストールする方法が列挙されているのでこちらを参考に進めます。
https://devcenter.heroku.com/articles/heroku-cli?source=post_page#download-and-install

ドキュメントに従い以下のコマンドをターミナルで実行。

```shell
$ sudo snap install --classic heroku
```

以下のような**エラー**が出ました。

```shell
error: cannot communicate with server: Post http://localhost/v2/snaps/heroku: dial unix /run/snapd.socket: connect: no such file or directory
```

どうやらWSL2では`snap`がうまく動作しない模様です。
そこで以下のコマンドでスタンドアローンインストールを実行します。

```shell
$ sudo curl https://cli-assets.heroku.com/install.sh | sh
```

### バージョン確認

インストールに成功したかを確認するために以下のコマンドでCLIのバージョンを表示してみます。

```shell
$ heroku --version heroku/7.47.11 linux-x64 node-v12.16.2
```

インストールに成功し、WSL2上にてHeroku CLIが使えるようになりました。

## デプロイする下準備

それではHeroku CLIを実際に動かしてデプロイする準備をしていきます。

### Herokuへログイン

以下のページからHerokuアカウントを登録します。
https://signup.heroku.com/

アカウントの登録ができたらターミナルからログインをしていきます。

```shell
$ heroku login
  heroku: Press any key to open up the browser to login or q to exit:
  # ブラウザにてログイン処理を行う
```

`q`以外のキーを押すと自動的にデフォルトのブラウザでHerokuのログイン画面が開かれるのでブラウザ上でアカウントへログインします。

以下の画面が表示されたら正常にログインできています。

![ログイン成功後の画面](/images/vueapp-deployto-heroku/image01.png)

```shell
Logging in... done
  Logged in as '自分のメールアドレス'
```

ターミナルでもログインが成功していることを確認できます。

### 必要なファイルの生成

以下のページにVue CLIで構築したアプリをHerokuへデプロイする方法が載っていたため参考にして進めていきます。
https://cli.vuejs.org/guide/deployment.html#heroku

デプロイするのにstatic.jsonというファイルが必要みたいなので、Vueプロジェクトのルートディレクトリに移動してstatic.jsonを作成します。

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

static.jsonはHerokuへのデプロイで必要になるためGitへ追加しておきます。

ここからは作成したアプリでGitコマンドが使える環境を前提に進めます。
Gitのインストール、`git init`でプロジェクトの初期化が済んでいる状態で進めてください。

先程作成したstatic.jsonをステージング。

```shell
$ git add static.json
```

コミットする。

```shell
$ git commit -m "add static configuration"
```

### Herokuアプリを作成

ターミナルからHerokuへログインしデプロイ対象のプロジェクトのディレクトリで以下のコマンドを実行します。

```shell
$ heroku create
Creating app... done
```

コマンド１つでHerokuプロジェクトが生成されます。
以下のコマンドでアプリのページを開くことができます。

```shell
$ heroku open
```

デフォルトのブラウザが起動してアプリのページが表示されれば正常にアプリが作成されています。

![アプリの初期ページを開いた様子](/images/vueapp-deployto-heroku/image02.png)

ブラウザからアクセスできることが確認できればOKです。

### アプリをビルドする環境を作成

Vueで作成したSPAはビルドする必要があります。
Heroku上でアプリをビルドするために以下のビルドパックが必要になるため、それぞれ追加します。
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
ビルドパックはheroku/Node.js → heroku-buildpack-staticの順に追加してください。追加する順番を間違うと上手く動作しない可能性があります。
:::

### Herokuへデプロイする

Herokuへデプロイしていきます。

以下のGitコマンドを実行します。

```shell
$ git push heroku main
```

すぐにHerokuへデプロイ処理が行われます。
このときにビルドも同時に行われ最新のdistディレクトリがデプロイされます。

:::message
ドキュメントではmasterブランチにプッシュしていますが、現在はmainブランチに変更されているため注意してください。
:::

## デプロイできたかの確認

ブラウザからアプリのページにアクセスして確認してみます。

以下のコマンドを実行することでデフォルトのブラウザでアプリを確認できます。

```shell
$ heroku open
```

![ブラウザからデプロイしたアプリを開いた様子](/images/vueapp-deployto-heroku/image03.png)

Vue CLIで作成したプロジェクトがブラウザに表示され動作が確認できました。

## さいごに

今回Herokuを使ってVueで作成したSPAのデプロイ手順をまとめました。
インターネット上で見る記事ではExpressを使用してデプロイしている例が多くありましたが、ビルドパックを使うことで簡単にビルド、静的ホスティングまで行ってくれるのに驚きました。

今では優良なホスティングサービスが多く存在するため、Heroku以外の選択肢もたくさんあります。今回久しぶりにデプロイを行う中でホスティングサービスの充実によってデプロイのハードルが下がってきていると感じました。

お役に立てれば幸いです。最後まで読んでいただきありがとうございました。

## 補足：デプロイ後にアプリ名を変更したい

デフォルトだとランダムでアプリの名前が付与されます。
変更したい場合はCLIを使い以下の手順で変更可能です。

```shell
$ heroku apps:rename 【変更したい名前】
```

アプリの名前が変わるとアドレスも変わります。
新しいアプリページは名前の変更後にダッシュボードか`heroku open`で開くことができます。
アプリのダッシュボードから変更するとGit remoteへ変更がうまく反映されないため、上記のコマンドを使用し、Heroku CLIから変更するのが無難です。

## 参考

- [The Heroku CLI | Heroku Dev Center](https://devcenter.heroku.com/articles/heroku-cli?source=post_page#download-and-install)
- [Deployment | Vue CLI](https://cli.vuejs.org/guide/deployment.html#heroku)
- [WSl 環境における Heroku CLI の導入 - Qiita](https://qiita.com/RoaaaA/items/604f7538d9ef57d2f9c7)
- [Vue.js のアプリケーションを手早く Heroku で公開する - Qiita](https://qiita.com/akirakudo/items/f63322f6851feef3d9e4)
