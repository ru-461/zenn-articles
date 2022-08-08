---
title: "Node.jsがv16でAppleSiliconに正式対応していた"
emoji: "🍎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nodejs", "applesilicon", "docker", "nodenv"]
published: true
---

# はじめに

Node.js v16系となるバージョンが2021年の4月20日にリリースされました。v14に次ぐ[LTS（Long Term Support）](https://nodejs.org/ja/about/releases/)にあたるバージョンになります。Node.jsはメジャーバージョンが偶数になるタイミングでLTSとなり、約30ヶ月間のサポートがされる形となっています。

またリリースしてすぐLTSのアクティブリリースになるわけではなく、約半年後にアクティブLTSリリースとなります。

そして今回リリースされた新のLTS、Node.js 16が**AppleSilicon を正式にサポートする最初のLTSバージョン**となります。つまりM1チップを始めとするSoCを搭載したMacBookやiMac上にて**ネイティブ動作するようになった**ということです。これはすごいですね。

AppleSiliconで動作するかどうかを掲載しているサイト[Does it ARM](https://doesitarm.com/)上でもv16以降で対応と更新されておりました。

https://doesitarm.com/app/nodejs/

公式サイトでも最新版のタブから、x64とARM64にマルチ対応したインストーラーがmacOSへ提供されるようになりました。マルチビルドを採用しているため、環境に応じてネイティブに動作します。

![v16から配布されるようになったインストーラーの画像](/images/node-supports-applesilicon/image01.png)

メジャーバージョンが上がったことで**Timers Promises API が安定版に移行**や、**V8 JavaScript エンジンが V8.9.0 へ更新**などと比較的大きな変更も含まれています。

Node.js v16のリリースノート（GitHub）はこちらになります。

https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V16.md

# さっそくインストールしてみる

それでは、Node.js 16を実際にAppleSiliconなMacへインストールしていきます。今回はローカルとDockerでそれぞれ検証しました。

この記事の内容は執筆時点でM1チップを搭載したMacbook Proにて検証したものになります。

## 検証環境

- MacBook Pro（macOS Big Sur 11.4）
- anyenv 1.1.2
- nodenv 1.4.0+3.631d0b6
- Docker Desktop v3.3.3

## ローカルへnodenvでインストール

まず、ローカルな環境にNode.jsをインストールします。公式サイトからバイナリをダウンロードしてきてもいいのですが、すでにanyenvを使ってnodeのバージョン切り替えができる環境を構築しているため、nodenvを使用します。

:::message
検証のためターミナルは情報を見るから、Rosettaを使用するのチェックを外した状態で使用しています。
:::

anyenvのインストールは[以前投稿したこちらの記事](https://zenn.dev/ryuu/articles/use-anyversions)で詳しく紹介しているのでこの記事では割愛します。

nodenv anyenv経由で以下のコマンドを使ってインストールできます。

```shell
# anyenvでnodenvをインストール
$ anyenv install nodenv

# シェルを再起動
$ exec $SHELL -l
```

最新のリリースに対応するため[anyenv update](https://github.com/znz/anyenv-update)という更新プラグインを使用し、最新の状態にアップデートしておきます。

```shell
$ anyenv update
```

アップデートが完了したらnodenvから最新のNode.jsをインストールします。記事執筆時点では **v16.2.0**がリリースされていたのでこちらを指定します。

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

Node.jsは公式でDockerイメージを配布しており、DockerHubから習得が可能です。

https://hub.docker.com/_/node/

確認したところNode.js v16のイメージが配布されていたのでDockerコンテナを作成して実行環境を確認します。

Docker Desktopも先日公開されたバージョンv3.3.1にてAppleSiliconへネイティブ対応しました。^[[Docker Desktop for Apple silicon | Docker Documentation](https://docs.docker.com/docker-for-mac/apple-silicon/)]DockerはインストーラやHomebrewで簡単にインストールできます。

```shell
# HomebrewでDockerをインストール
$ brew install --cask docker

# Docker-Desktopを起動
$ open /Applications/Docker.app

# バージョンを確認
$ docker --version
  Docker version 20.10.6, build 370c289
```

続けて以下のコマンドを叩くとNode.js v16のインストールされたコンテナが起動します。

```docker
# DockerHubからNode.js 16の公式イメージを取得して実行
$ docker run --rm -it node:16 /bin/bash
```

`docker run`とすることでイメージのダウンロードとコンテナの起動を同時に行うことができます。今回はローカルにイメージがないため、DockerHubから取得する形となります。Node.jsのイメージをダウンロードするため初回起動には少し時間がかかります。alpineやslimなどを使うことでさらに軽量化も可能です。

そしてコンテナを起動するときにオプションでコマンドを実行しコンテナの内部に繋いでbashを起動します。

```docker
# コンテナに入った状態
root@:/#
```

コンテナが起動し、接続できたのでNode.jsのバージョンを確認してみます。

```docker
root@:/# node -v
v16.2.0
```

しっかりとNode.js v16がインストールされていることが確認できました。Dockerを使うことで最新のバージョンを試したいときも手軽にコンテナを作成して試せるのが便利ですね。

上のコマンドで作成したNode.jsコンテナを止めるときは`exit`にて止めることができます。またオプションに`--rm`を指定しているため、起動と同時にコンテナは破棄されます。イメージはローカルに残るため、次回起動するときはさらに高速で環境を立ち上げることが可能です。

# おわりに

今まで開発にLTS版のNode.js v14系を使用していましたが、あっという間にバージョンが16まで進んでいて驚きました。AppleSilicon正式にネイティブ対応したことでアーキテクチャ問題を解決し、Rosetta2まわりを考慮することなくインストールできるので心理的負担が減りました。

Dockerをはじめとする他の技術もAppleSiliconへの対応がかなり進んできて、AppleSiliconの登場時に比べるとベストプラクティスが確立し、柔軟な開発環境が構築可能になりました。開発環境が整備されつつあるAppleSiliconの今後の展開にも更に期待したいところです。

最後まで読んでいただきありがとうございました。
