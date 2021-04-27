---
title: "さらに高速化したaptを使おう [apt-fast]"
emoji: "🌊"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["linux","apt","ubuntu"]
published: false
---

# はじめに

Ubuntu をはじめとする Debian 系の OS ではパッケージマネージャーとして apt がおもに使用されます。新しいパッケージをインストールする際に `apt update コマンド`を叩いて最新の状態にしておくことが公式から推奨されており、日常的に使うことがおおいコマンドになります。Ubuntu でパッケージをインストールしようとしたときにしばらく時間が経つとパッケージのアップデートやリポジトリの更新が溜まっており、処理が終わるまでかなり待たされた経験がある人も多いのではないでしょうか。そこで今回は apt コマンドを更に高速化させて利用できるラッパーツール「apt-fast」を紹介します。

GitHub リポジトリは以下になります。
https://github.com/ilikenwf/apt-fast

# インストール

GitHub リポジトリの ReadMe にもありますが、インストールはとても簡単でターミナルから以下のコマンドを順番に実行するだけで使用できるようになります。

```shell
# リポジトリを追加
$ sudo add-apt-repository ppa:apt-fast/stable

# リポジトリを最新に更新
$ sudo apt-get update

# apt-fastをインストール
$ sudo apt-get -y install apt-fast
```


# おわりに