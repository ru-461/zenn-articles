---
title: "速さを追い求めて apt から apt-fast へ乗り換えてみた"
emoji: "🌊"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["linux","apt","ubuntu"]
published: false
---

# はじめに

Ubuntu をはじめとする Debian 系の OS ではパッケージマネージャーとして `apt` がおもに使用されます。新しいパッケージをインストールする際に `apt update コマンド`を利用してリポジトリを最新の状態にしておくことを Ubuntu 公式が推奨しており、Ubuntu を使う中で使用頻度が高いコマンドとなります。Ubuntu でパッケージをインストールしようとしたときにしばらく時間が経つとパッケージのアップデートやリポジトリの更新が溜まっており、処理が終わるまでかなり待たされた経験がある人も多いのではないでしょうか。apt を使ったインストールやアップデートの速度を早くする方法として、ミラーサーバーを見直す、ネットワークの接続状況を見直すといったことが挙げられます。

そこでツールを利用して速度をあげられる方法をないか探したところ「apt-fast」といったツールを発見しました。今回は、apt から apt-fast へ乗り換えて速度を爆速にしてみたのでまとめます。

apt-fast の GitHub リポジトリは以下になります。
https://github.com/ilikenwf/apt-fast

# インストール

## 準備

今回は Ubuntu20.04 LTS をインストールした WSL2 の環境に導入しました。他のディストリビューションの場合は GitHub リポジトリの README を適宜参考にインストールしてください。

GitHub リポジトリの README にもありますが、インストール自体はとても簡単でターミナルから以下のコマンドを順番に実行するだけです。
まず apt-fast はデフォルトのリポジトリに含まれておらずそのままではインストールできないため、下準備として apt-fast があるリポジトリを追加しておく必要があります。

```shell
# apt-fastを含むリポジトリを追加
$ sudo add-apt-repository ppa:apt-fast/stable

  This PPA contains tested (stable) builds of apt-fast.
  More info: https://launchpad.net/~apt-fast/+archive/ubuntu/stable
  Press [ENTER] to continue or Ctrl-c to cancel adding it. # Enterで続行
  ...

  Reading package lists... Done

# リポジトリを最新に更新
$ sudo apt-get update
```
## インストールと初期設定

リポジトリの追加と更新が終わったら以下のコマンドで apt-fast をインストールします。

```shell
# apt-fastをインストール
$ sudo apt-get -y install apt-fast
```

インストールがはじまります。Ubuntu だと途中で以下のような設定画面が表示されます。

![パッケージマネージャー選択画面の画像](https://storage.googleapis.com/zenn-user-upload/s9o7gsvs6amu9g0pjfzbwjxgy9r9)
***使用するパッケージマネージャーの選択***

使用するパッケージマネージャーを選択します。apt-fast は `apt-get`、`apt`、`aptitude` の 3 つをデフォルトでをサポートしています。パッケージマネージャーとして apt を使いたいため選択肢から apt を選択します。

![最大の接続数を指定する画面の画像](https://storage.googleapis.com/zenn-user-upload/qdskjagjuzn7ctb2h18tgdkl5496)
***最大接続数の指定***

aria2c が使用する接続数の最大を指定します。デフォルトでも十分速いのでそのままで設定します。
![ダイアログ表示選択の画像](https://storage.googleapis.com/zenn-user-upload/ek9a8751nq9req9s1trr0mi5yx4i)
*** ダウンロード前に確認ダイアログを表示するかどうかの選択 ***

上記の画面から対話形式でパッケージマネージャーや最大コネクション数などを指定できます。ここで聞かれる質問の内容や、詳しい設定についてはあとから編集可能です。

apt-fast の設定は、`/etc/apt-fast.conf`に記述されています。設定ファイルを直接編集することで使いやすいように設定を変更できます。
設定できる内容はドキュメントに詳しく記載されているため、覗いてみてください。ミラーサーバーの設定など細かく設定できるようです。

デフォルトの設定でも十分に爆速なので無理に触る必要はありません。

# おわりに