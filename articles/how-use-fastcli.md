---
title: "ターミナルから爆速で回線速度を計測する【fast-cli】"
emoji: "🌠"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["nodejs", "npm", "yarn", "ツール"]
published: true
---

## はじめに

昨今、インターネット上には回線の速度を簡単に測定できるサービスが多数存在しています。
その1つに、Netflixが提供している「Fast」というサービスがあります
https://fast.com/ja/

このページにアクセスすると、接続中の回線速度を自動で測定してくれます。

そこで、回線速度を測定する際に、毎回ブラウザを開いて測定サイトにアクセスする手間を省くため、ターミナルからインターネット回線速度を測定できるツールを紹介します。

## コマンドラインツール「fast-cli」

[Node.js](https://nodejs.org)がインストールされている場合、コマンド`npx`を利用してインストールせずに使用できます。

npxについては過去に投稿した記事で詳しく説明しているので興味がある方は併せてご覧ください。
https://zenn.dev/ryuu/articles/what-npxcommand

また、グローバルに`npm install --global fast-cli`としてインストールして使用もできます。
グローバルにインストールして使う場合は`fast-cli`を`fast`に適宜読み替えて実行してください。

## 実行

### ダウンロードとアップロード速度を計測

ダウンロードの速度を計測します。

```shell
$ npx fast-cli
```

オプション`--upload`を付けないとダウンロードのみの計測となります。
`--upload`は`-u`と入力しても同じ意味となり問題なく動作します。

```shell
# ダウンロード速度とアップロード速度を計測
$ npx fast-cli --upload
```

### ダウンロード速度を計測して結果を1行表示

```shell
$ npx fast-cli --single-line
```

1行でシンプルにダウンロード速度を計測します。

### fast-cliのヘルプを表示

```shell
$ npx fast-cli --help
```

使用可能なコマンドと使用例を表示します。`--help`は`-h`としても同じ意味となります。

## おわりに

Fastを開かずとも、ターミナルから簡単に回線の速度を図れるのは便利です。

Discussionにて紹介していただいたのですが、同じようなツールとして[Speedtest CLI](https://www.speedtest.net/ja/apps/cli)があります。
こちらの導入したときのメモは[スクラップ](https://zenn.dev/ryuu/scraps/2d4e2592dbe45d)にまとめております。併せてご覧ください。

最後まで読んでいただきありがとうございました。

## 参考

- [sindresorhus / fast-cli](https://github.com/sindresorhus/fast-cli)
- [回線速度のテストができる「fast」コマンドが便利だった](https://qiita.com/suin/items/8398f0b07299a3cc194f)
- [windows10 fast.comのコマンドラインツール「fast-cli」を使って回線速度を計測する](https://mebee.info/2020/04/28/post-10023)
