---
title: "VSCode使いなら抑えておくべきCodeコマンド"
emoji: "🧩"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vscode","windows","mac"]
published: true
---

# はじめに

ターミナルでファイルを操作するとき、現在作業しているディレクトリやファイルを VSCode で直接開きたい場面があります。VSCode を起動してフォルダ選択、ファイル選択して開きますが、それだと少し手間がかかってしまいます。

そこで VSCode が用意している `Code コマンド`を使うことでターミナルから直接 VSCode を開けるようになります。

VSCode をメインに使っているならぜひとも抑えておきたいコマンドなので紹介します。

# Codeコマンドとは

VSCode には `Code コマンド` と呼ばれるコマンドが存在します。このコマンドを使うことでターミナルから直接 VSCode を起動したり、指定したファイル、ディレクトリを瞬間的に開けるようになります。

瞬間的に VSCode が立ち上がり、ファイル・ディレクトリが開かれるので、生産性が上がります。

# Codeコマンドの準備【Windows】

VSCode をインストールすることで自動的にパスが通りコマンドプロンプトから使用可能になります。
コマンドプロンプトから以下のコマンドで VSCode が起動します。
```shell
$ code
```

ローカルのファイルを開くことはもちろん可能ですが、WSL であっても同じ用に使用できます。
VSCode と WSL2 の親和性が高いので Windows で開発するときに威力を発揮します。
# Code コマンドの準備【MacOS】

MacOS の場合はシェルコマンドを手動で実行する必要があり、少し手数を踏む必要があります。
VSCode を開き、`⌘ + shift + p` でコマンドパレットを表示します。

コマンドパレットに `shell command` と打ち込むことで以下のような選択が表示されます。

![コマンドパレットを開いた様子](https://storage.googleapis.com/zenn-user-upload/g6g74ukk33yndvsz0siedopguxrm)
*シェルコマンド : PATH内にcodeコマンドをインストールしますを選択*

管理者特権でコマンドをインストールするのを許可する必要があるため OK を選択します。
![ダイアログが表示される様子](https://storage.googleapis.com/zenn-user-upload/kuoa8f1u9ok94qdg5hytbhhbyd3c =200x)

コマンドのインストールが完了すると右下にメッセージが表示されます。これでパスが通って Code コマンドがターミナル上で使用可能になりました。

ターミナルを開いて code と打ってみましょう。VSCode が起動します。

```shell
$ code
```

これで Code コマンドのインストール、使用する準備は完了です。

Code コマンドは VSCode Stable 版、Insiders 版ともに使用可能です。2 つのビルドは共存できるため、コマンドも共存可能となります。

VSCode Insiders のコマンドは以下のようになります。
```shell
$ code-insiders
```
使うにはそれぞれのビルドでコマンドパレットからコマンドインストールの手順を踏む必要があります。
コマンドが独立していることで使いたい VSCode を選ぶことができるのは便利です。

# コマンド使用例

よく使うコマンドをまとめました。コマンドは Windows と Mac 共通で同じものが使えます。

:::message
VSCode Insiders の場合は `code` を `code-insiders` に適宜置き換えてください。
:::
## VSCodeを起動

```shell
$ code
```

## VSCodeでディレクトリを開く

```shell
$ code .
```
開きたいディレクトリへ移動して以下のコマンドを実行します。

## VSCodeでファイルを開く

```shell
$ code hoge.txt
```

## VSCodeでファイルを比較する

```shell
$ code -d hoge1.txt hoge2.txt
```

## VSCodeのバージョンを表示

```shell
$ code -v
```
## ヘルプを表示

```shell
$ code -h
```
使えるコマンドが一覧で表示されます。

# おわりに

Code コマンドを利用することでターミナルから VSCode でファイルやディレクトリを直接開けるようになるため、結果として生産性の向上に繋がります。
VSCode をメインのエディタとして使うなら是非とも抑えておきたいコマンドだと感じます。

Code コマンドを上手く活用して生産性を上げていきましょう。

最後まで読んでいただきありがとうございました。

# 参考ドキュメント

https://code.visualstudio.com/docs/editor/command-line