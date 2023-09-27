---
title: "ターミナルから爆速で回線速度を計測する【fast-cli】"
emoji: "🌠"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["nodejs", "npm", "yarn", "ツール"]
published: true
---

## はじめに

昨今、インターネット上には回線の速度を簡単に測定できるサービスが多く存在しています。
その中に、Netflixが展開しているFAST.comという便利なサービスがあります。

https://fast.com/ja/

上記リンクにアクセスすると接続している回線の速度を測定してくれます。
そこで、回線速度を計測したいときにブラウザを使って測定サイトにアクセスしなくても、ターミナルからFast.comを経由して爆速でインターネット回線速度を図る方法が便利だったためまとめてみました。

## 公式コマンドラインツール「fast-cli」のインストール

Node.js（バージョン8以降）がインストールされている場合、npmやYarnなどのパッケージマネージャーから簡単に導入できます。以下のコマンドをターミナルにて実行しグローバルにインストールすることで使えるようになります。

### npmでインストール

```shell
$ npm install --global fast-cli
```

### Yarnでインストール

```shell
$ yarn global add fast-cli
```

## 実行

### ダウンロード速度の計測

```shell
$ fast
```

上のコマンドで回線のダウンロードの速度計測が始まります。オプションとして`--upload , -u`を付けないとダウンロードのみの計測となります。

### ダウンロードとアップロード速度を計測

```shell
$ fast --upload
```

ダウンロード速度とアップロード速度をそれぞれ計測します。

### ダウンロード速度を計測して結果を一行表示

```shell
$ fast --single-line
```

一行でシンプルにダウンロード速度を計測します。

### fast-cliのヘルプを表示

```shell
$ fast --help
```

使用可能なコマンドと使用例を表示します。

## おわりに

ターミナルから簡単に回線の速度を図れるのは便利です。公式のコマンドラインツールということもあり、計測も安定していて精度も良さげです。是非活用してみてください。

Discussionにて紹介されたのですが、同じようなツールとして[Speedtest CLI](https://www.speedtest.net/ja/apps/cli)があります。こちらの導入したときのメモは[スクラップ](https://zenn.dev/ryuu/scraps/2d4e2592dbe45d)にまとめております。合わせてご覧ください。

最後まで読んでいただきありがとうございました。

## 参考

- [sindresorhus / fast-cli](https://github.com/sindresorhus/fast-cli)
- [回線速度のテストができる「fast」コマンドが便利だった](https://qiita.com/suin/items/8398f0b07299a3cc194f)
- [windows10 fast.com のコマンドラインツール「fast-cli」を使って回線速度を計測する](https://mebee.info/2020/04/28/post-10023)
