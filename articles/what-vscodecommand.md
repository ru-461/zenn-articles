---
title: "VSCode使いなら抑えておくべきCodeコマンド"
emoji: "🧩"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vscode","windows","mac"]
published: true
---

# はじめに

ターミナルでファイルを操作するとき、現在作業しているディレクトリやファイルをVSCodeで直接開きたい場面があります。VSCodeを起動してフォルダ選択、ファイル選択して開きますが、それだと少し手間がかかってしまいます。

そこでVSCodeが用意している`Code コマンド`を使うことでターミナルから直接VSCodeを開けるようになります。

VSCodeをメインに使っているならぜひとも抑えておきたいコマンドなので紹介します。

# Codeコマンドとは

VSCodeには`Code コマンド`と呼ばれるコマンドが存在します。このコマンドを使うことでターミナルから直接VSCodeを起動したり、指定したファイル、ディレクトリを瞬間的に開けるようになります。

瞬間的にVSCodeが立ち上がり、ファイル・ディレクトリが開かれるので、生産性が上がります。

# Codeコマンドの準備【Windows】

VSCodeをインストールすることで自動的にパスが通りコマンドプロンプトから使用可能になります。
コマンドプロンプトから以下のコマンドでVSCodeが起動します。

```shell
$ code
```

ローカルのファイルを開くことはもちろん可能ですが、WSLであっても同じ用に使用できます。
VSCodeとWSL2の親和性が高いのでWindowsで開発するときに威力を発揮します。

# Code コマンドの準備【macOS】

macOSの場合はシェルコマンドを手動で実行する必要があり、少し手数を踏む必要があります。
VSCodeを開き、`⌘ + shift + p`でコマンドパレットを表示します。

コマンドパレットに`shell command`と打ち込むことで以下のような選択が表示されます。

![コマンドパレットを開いた様子](https://storage.googleapis.com/zenn-user-upload/g6g74ukk33yndvsz0siedopguxrm)
*シェルコマンド : PATH内にcodeコマンドをインストールしますを選択*

管理者特権でコマンドをインストールするのを許可する必要があるためOKを選択します。

![ダイアログが表示される様子](https://storage.googleapis.com/zenn-user-upload/kuoa8f1u9ok94qdg5hytbhhbyd3c=200x)

コマンドのインストールが完了すると右下にメッセージが表示されます。これでパスが通ってCodeコマンドがターミナル上で使用可能になりました。

ターミナルを開いてcodeと打ってみましょう。VSCodeが起動します。

```shell
$ code
```

これでCodeコマンドのインストール、使用する準備は完了です。

CodeコマンドはVSCode Stable版、Insiders版ともに使用可能です。2つのビルドは共存できるため、コマンドも共存可能となります。

VSCode Insidersのコマンドは以下のようになります。

```shell
$ code-insiders
```

使うにはそれぞれのビルドでコマンドパレットからコマンドインストールの手順を踏む必要があります。
コマンドが独立していることで使いたいVSCodeを選ぶことができるのは便利です。

# コマンド使用例

よく使うコマンドをまとめました。コマンドはWindowsとMac共通で同じものが使えます。

:::message
VSCode Insidersの場合は`code`を`code-insiders`に適宜置き換えてください。
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

Codeコマンドを利用することでターミナルからVSCodeでファイルやディレクトリを直接開けるようになるため、結果として生産性の向上に繋がります。
VSCodeをメインのエディタとして使うなら是非とも抑えておきたいコマンドだと感じます。

Codeコマンドを上手く活用して生産性を上げていきましょう。

最後まで読んでいただきありがとうございました。

# 参考

- [The Visual Studio Code command-line options](https://code.visualstudio.com/docs/editor/command-line)