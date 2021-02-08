---
title: "WSL2にNode.jsとYarnを導入する"
emoji: "📥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["wsl2", "yarn", "nodejs"]
published: false
---

# Yarnとは？

`Yarn` とは Node.js 用のパッケージマネージャーです。

Node.js をインストールすると標準で `npm(Node Package Manager)` というパッケージマネージャーが付属します。
Node.js 向けのパッケージマネージャとしては `npm` が有名ですが、他にもパッケージマネージャーはいくつか存在します。
その中の１つに FaceBook 社が開発した Yarn というパッケージマネージャーがあります。

公式ページ
https://yarnpkg.com/

今回は Node.js をインストールしてある WSL2(Windows Sub System for Linux 2)の環境に Yarn を導入して動かすところまで行います。

# インストール
## Node.jsのインストール

まず apt を使って node.js とその周りの依存関係をまとめてインストールします。
ターミナルにて以下のコマンドを実行すると node.js がインストールされます。

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

n で node.js がうまくインストールされているかを確認します。

```shell
$ node -v

v14.15.4
```
バージョンが返ってきたら WSL2 に Node.js がインストールされています。

