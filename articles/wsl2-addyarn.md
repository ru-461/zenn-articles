---
title: "WSL2にNode.jsとYarnを導入する"
emoji: "📥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["wsl2", "yarn", "nodejs","npm"]
published: true
---

## Yarnとは？

YarnとはJavaScript用のパッケージマネージャーです。

Node.jsをインストールすると標準で[npm](https://www.npmjs.com)というパッケージマネージャーが付属します。
パッケージマネージャとしてはnpmが有名ですが、他にもパッケージマネージャーはいくつか存在します。
その中の１つにFaceBook社が開発したYarnというパッケージマネージャーがあります。

こちらがYarnの公式ページになります。

https://yarnpkg.com/

Yarnはnpmが問題としていた部分を抑えるように設計され、登場当時はnpmよりインストール速度が早くなったりとメリットが大きかったようです。
今ではnpmのバージョンアップで差があまりなくなってきたとのこと。

JavaScript用パッケージマネージャーについて詳しく解説してくれている記事がありました。

https://zenn.dev/hibikine/articles/27621a7f95e761

優良な解説記事ありがとうございます！

今回はWSL2（Windows Sub System for Linux 2）の環境にNode.jsとYarnを導入して動かすところまで行います。

:::message
現在、Yarnにはv1とv2が存在します。この記事内ではYarn v1のインストール手順にについて解説します。
:::

## 環境

- Windows10 バージョン20H2
- WSL2（Ubuntu 20.04 LTS）

## インストール
### Node.jsのインストール

まずaptを使ってNode.jsとその周りの依存関係をまとめてインストールします。
ターミナルにて以下のコマンドを実行するとNode.jsがインストールされます。

```shell
$ sudo apt install -y nodejs npm
```

Node.js用のバージョン管理ツールnをWSL2に導入します。
バージョン管理ツールを使うことでプロジェクトごとNode.jsのバージョンを簡単に切り替えられるため重宝します。

```shell
$ sudo npm install -g n
```

続いてNode.jsの安定版（記事執筆時点ではv14.15.4 LTS）をインストールします。

```shell
$ sudo n lts
```

以後はnを使ってバージョン管理するため、最初にaptでインストールしたNode.jsとnpmを削除します。

```shell
$ sudo apt purge -y nodejs npm
```

依存関係も削除します。

```shell
$ sudo apt autoremove -y
```

nでNode.jsがうまくインストールされているかを確認します。

```shell
$ node -v

  v14.15.4
```
バージョンが返ってきたらWSL2にNode.jsがインストールされています。

### Yarnのインストール

つづいてYarnをインストールしていきます。Yarnのインストール方法は複数あります。

### aptでインストール

npmがインストールされていなくてもインストール可能な方法です。

まずリポジトリを追加します。
リポジトリの追加にcurlを使うため、予めcurlコマンドをインストールしておきます。

```shell
$ sudo apt install -y curl

# コマンドの確認
$ which curl
  /usr/bin/curl
```

curlが使えるようになったら以下のコマンドでリポジトリを追加します。

:::message
aptでインストールする際は以下のリポジトリ追加と構成を必ず行ってください。
リポジトリを更新しないとうまくインストールできない可能性があります。
:::

```shell
$ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
$ echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```

リポジトリを追加したら以下のコマンドを実行します。

```shell
$ sudo apt update && sudo apt install yarn
```

Yarnがインストールされたか確認します。

```shell
$ yarn --version
  1.22.5
```

### npmを使ってインストール

```shell
$ sudo npm install -g yarn
```

インストールされたバージョンを確認します。

```shell
$ yarn --version

  1.22.10
```

npmを使って導入する場合、グローバルオプション`-g`を付けてグローバルにインストールすることが公式ドキュメントにて推奨されています。

## さいごに

WSL2にNode.jsとYarnをインストールする方法についてまとめました。
パッケージマネージャーは依存性を解決しながらモジュールを管理してくれる優秀なツールで、JavaScriptの開発をする上では必要不可欠です。
今回の記事がWSL2を使ってJavaScript開発を始めるかたの参考になれば幸いです。

この記事内では触れませんでしたが、今現在はYarn v2というバージョンが開発中とのことなので今後調べて触ってみたいです。

最後まで読んでいただきありがとうございました。

## 参考

- [npmとyarnとpnpmの違い2021](https://zenn.dev/hibikine/articles/27621a7f95e761)
- [ubuntu 18.04 にyarn をインストールする - Qiita](https://qiita.com/crash-boy/items/5c9b7341e95b142e0d56)
- [Installation | Yarn](https://classic.yarnpkg.com/en/docs/install/#debian-stable)
- [tj/n: Node version management](https://github.com/tj/n)
