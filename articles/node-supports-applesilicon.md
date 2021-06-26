---
title: "Node.js が v16 で AppleSilicon に正式対応していた"
emoji: "🍎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nodejs", "applesilicon", "docker", "nodenv"]
published: true
---

# はじめに

Node.js v16 系となるバージョンが 2021 年の 4 月 20 日にリリースされました。v14 に次ぐ [LTS（Long Term Support）](https://nodejs.org/ja/about/releases/)となるバージョンになります。Node.js はメジャーバージョンが偶数になるタイミングで LTS となり、約 30 ヶ月間のサポートがされる形となっています。

またリリースしてすぐ LTS のアクティブリリースになるわけではなく、約半年後にアクティブ LTS リリースとなります。

そして今回リリースされた新の LTS、Node.js 16 が **AppleSilicon を正式にサポートする最初のLTSバージョン**となります。つまり M1 チップを始めとする SoC を搭載した MacBook や iMac 上にて**ネイティブ動作するようになった**ということです。これはすごいですね。

AppleSilicon で動作するかどうかを掲載しているサイト [Does it ARM](https://doesitarm.com/) 上でも v16 以降で対応と更新されておりました。

https://doesitarm.com/app/nodejs/

公式サイトでも最新版のタブから、x64 と ARM64 にマルチ対応したインストーラーが macOS へ提供されるようになりました。マルチビルドを採用しているため、環境に応じてネイティブに動作します。

![v16から配布されるようになったインストーラーの画像](https://storage.googleapis.com/zenn-user-upload/a9c810cafefa4e2a4dae41a3.png)

またメジャーバージョンが上がったことで Node.js 15 にて実装された **Timers Promises API が安定版に移行**したことや、**V8 JavaScript エンジンが V89.0 に更新**されたりなど大きな変更も含んでいます。

Node.js v16 のリリースノート（GitHub）はこちらになります。

https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V16.md

# さっそくインストールしてみる

それでは、Node.js 16 を実際に AppleSilicon な Mac へインストールしていきます。今回はローカルと Docker でそれぞれ検証しました。

この記事の内容は執筆時点で M1 チップを搭載した Macbook Pro にて検証したものになります。

## 検証環境

- MacBook Pro（macOS Big Sur 11.4）
- anyenv 1.1.2
- nodenv 1.4.0+3.631d0b6
- docker desktop v3.3.3

## ローカルへnodenvでインストール

まず、ローカルな環境に Node.js をインストールします。公式サイトからバイナリをダウンロードしてきてもいいのですが、すでに anyenv を使って node のバージョン切り替えができる環境を構築しているため、nodenv を使用します。

:::message
検証のためターミナルは `情報を見る` から、 `Rosettaを使用する` のチェックを外した状態で使用しています。
:::

anyenv のインストールは[以前投稿したこちらの記事](https://zenn.dev/ryuu/articles/use-anyversions)で詳しく紹介しているのでこの記事では割愛します。

nodenv anyenv 経由で以下のコマンドを使ってインストールできます。

```shell
# anyenvでnodenvをインストール
$ anyenv install nodenv

# シェルを再起動
$ exec $SHELL -l
```

最新のリリースに対応するため[anyenv update](https://github.com/znz/anyenv-update) という更新プラグインを使用し、最新の状態にアップデートしておきます。

```shell
$ anyenv update
```

アップデートが完了したら nodenv から最新の Node.js をインストールします。記事執筆時点では **v16.2.0** がリリースされていたのでこちらを指定します。

```shell
# 最新のNode.jsをインストール
$ nodenv install 16.2.0

# nodenのrehash
$ nodenv rehash

# バージョン確認
$ node -v
  v16.20.0
```

バージョンの表示ができれば成功です。

## Dockerでさくっと実行環境を作る

Node.js は公式で Docker イメージを配布しており、DockerHub から習得が可能です。

https://hub.docker.com/_/node/

確認したところ Node.js v16 イメージが配布されていたので Docker コンテナを作成して実行環境を確認します。

Docker-Desktop も先日公開されたバージョン v3.3.1 にて AppleSilicon にネイティブ対応しました。^[[Docker Desktop for Apple silicon | Docker Documentation](https://docs.docker.com/docker-for-mac/apple-silicon/)]Docker は公式サイトからインストーラーを利用してインストールするか、Homebrew を使用してインストールできます。

```shell
# HomebrewでDockerをインストール
$ brew install --cask docker

# Docker-Desktopを起動
$ open /Applications/Docker.app

# バージョンを確認
$ docker --version
  Docker version 20.10.6, build 370c289
```

Docker の実行環境ができたら以下のコマンドを叩くだけで Node.js 16 がインストールされたコンテナが起動します。

```docker
# DockerHubからNode.js 16の公式イメージを取得して実行
$ docker run --rm -it node:16 /bin/bash
```

`docker run` とすることでイメージのダウンロードとコンテナの起動を同時に行うことができます。今回はローカルにイメージがないため、DockerHub から取得する形となります。Node.js のイメージをダウンロードするため初回起動には少し時間がかかります。alpine や slim などを使うことでさらに軽量化も可能です。

そしてコンテナを起動するときにオプションでコマンドを実行しコンテナの内部に繋いで bash を起動します。

```docker
# コンテナに入った状態
root@:/#
```

コンテナが起動し、接続できたので Node.js のバージョンを確認してみます。

```docker
root@:/# node -v
v16.2.0
```

しっかりと Node.js v16 がインストールされていることが確認できました。Docker を使うことで最新のバージョンを試したいときも手軽にコンテナを作成して試せるのが便利ですね。

上のコマンドで作成した Node.js コンテナを止めるときは `exit` にて止めることができます。`--rm` を指定しているため、起動と同時にコンテナは破棄されます。イメージはローカルに残るため、次回起動するときはさらに高速で環境を立ち上げることが可能です。

# おわりに

今まで開発に LTS 版だった Node.js v14 系を使用していましたが、あっという間にバージョンが 16 まで進んでいて驚きました。AppleSilicon 正式にネイティブ対応したことで AppleSilicon 黎明期に頻出していたアーキテクチャ問題、Rosetta2 まわりを考慮することなくインストールできるので心理的負担が減りました。

Docker をはじめとする他の技術も AppleSilicon への対応がかなり進んできて、AppleSilicon の登場時に比べるとベストプラクティスが確立し、柔軟な開発環境が構築可能になりました。開発環境が整備されつつある AppleSilicon の今後の展開にも更に期待したいところです。

最後まで読んでいただきありがとうございました。