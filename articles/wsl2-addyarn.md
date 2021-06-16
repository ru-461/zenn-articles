---
title: "WSL2 に Node.js と Yarnを導入する"
emoji: "📥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["wsl2", "yarn", "nodejs","npm"]
published: true
---

# Yarnとは？

`Yarn` とは JavaScript 用のパッケージマネージャーです。

Node.js をインストールすると標準で `npm(Node Package Manager)` というパッケージマネージャーが付属します。
パッケージマネージャとしては `npm` が有名ですが、他にもパッケージマネージャーはいくつか存在します。
その中の１つに FaceBook 社が開発した Yarn というパッケージマネージャーがあります。

こちらが Yarn の公式ページになります。

https://yarnpkg.com/

Yarn は npm が問題としていた部分を抑えるように設計され、登場当時は npm よりインストール速度が早くなったりとメリットが大きかったようです。
今では npm のバージョンアップで差があまりなくなってきたとのこと。

JavaScript 用パッケージマネージャーについて詳しく解説してくれている記事がありました。

https://zenn.dev/hibikine/articles/27621a7f95e761

優良な解説記事ありがとうございます！

今回は WSL2(Windows Sub System for Linux 2)の環境に Node.js と Yarn を導入して動かすところまで行います。

:::message
現在、Yarn には `V1` と `V2` が存在します。この記事内では Yarn V1 のインストール手順にについて解説します。
:::

# 環境

- Windows10 バージョン 20H2
- WSL2 (Ubuntu 20.04.1 LTS (Focal Fossa))

# インストール
## Node.jsのインストール

まず apt を使って Node.js とその周りの依存関係をまとめてインストールします。
ターミナルにて以下のコマンドを実行すると Node.js がインストールされます。

```shell
$ sudo apt install -y nodejs npm
```

Node.js 用のバージョン管理ツール `n` を WSL2 に導入します。
バージョン管理ツールを使うことでプロジェクトごと Node.js のバージョンを簡単に切り替えられるため重宝します。

```shell
$ sudo npm install -g n
```

続いて Node.js の安定版(記事執筆時点では v14.15.4(LTS))をインストールします。

```shell
$ sudo n lts
```

以後は `n` を使ってバージョン管理するため、最初に apt でインストールした Node.js と npm を削除します。

```shell
$ sudo apt purge -y nodejs npm
```

依存関係も削除します。

```shell
$ sudo apt autoremove -y
```

n で Node.js がうまくインストールされているかを確認します。

```shell
$ node -v

  v14.15.4
```
バージョンが返ってきたら WSL2 に Node.js がインストールされています。

## Yarnのインストール

つづいて Yarn を使いたいため、インストールしていきます。Yarn のインストール方法は複数あります。

## aptでインストール

npm がインストールされていなくてもインストール可能な方法です。


まずリポジトリを追加します。
リポジトリの追加に Curl を使うため、予め Curl コマンドをインストールしておきます。

```shell
$ sudo apt install -y curl

# コマンドの確認
$ which curl
  /usr/bin/curl
```

curl が使えるようになったら以下のコマンドでリポジトリを追加します。

:::message
apt でインストールする際は以下のリポジトリ追加と構成を必ず行ってください。
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

Yarn がインストールされたか確認します。

```shell
$ yarn --version
  1.22.5
```

## npmを使ってインストール

```shell
$ sudo npm install -g yarn
```

インストールされたバージョンを確認します。

```shell
$ yarn --version

  1.22.10
```
npm を使って導入する場合、グローバルオプション(-g) を付けてグローバルにインストールすることが公式ドキュメントにて推奨されています。

# さいごに

WSL2 に Node.js と yarn をインストールする方法についてまとめました。
パッケージマネージャーは依存性を解決しながらモジュールを管理してくれる優秀なツールで、JavaScript の開発をする上では必要不可欠です。
今回の記事が WSL2 を使って JavaScript 開発を始めるかたの参考になれば幸いです。

この記事内では触れませんでしたが、今現在は `yarn v2` というバージョンが開発中とのことなので今後調べて触ってみたいです。

最後まで読んでいただきありがとうございました。

# 参考

- [npmとyarnとpnpmの違い2021](https://zenn.dev/hibikine/articles/27621a7f95e761)
- [ubuntu 18.04 にyarn をインストールする - Qiita](https://qiita.com/crash-boy/items/5c9b7341e95b142e0d56)
- [Installation | Yarn](https://classic.yarnpkg.com/en/docs/install/#debian-stable)
[tj/n: Node version management](https://github.com/tj/n)
