---
title: "【macOS Monterey】HomebrewでPHPの実行環境をセットアップする"
emoji: "🐘"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["mac", "php", "homebrew"]
published: false
---

# はじめに

macOS 12ことMontereyがリリースされてしばらく日が経ちました。ユニバーサルコントロール対応などの目玉機能が注目される中密かにPHPがmacOSにバンドルされなくなっていることに気づきます。
いままでのmacOSではインストール時からPHPがセットアップされて使える状態になっているというのが暗黙の了解で常識化していたように感じます。

12 へアップグレードしたmacOSで`php --version`をすると、`command not found`と言われてしまいます。どうやらPHPは含まれなくなったようです。

そこでmacOSでパッケージマネージャHomebrewを使用してPHPの実行環境をセットアップするまでにやったことまとめてみます。恥ずかしながらかなりハマったポイントがあるためこれから新規にPHPをmacOS 12以降でセットアップする方の参考になれば嬉しいです。

# 使用環境

・Macbook Pro(13-inchi, M1, 2020)
・macOS Monterey(バージョン 12.3)
・Homebrew/homebrew-core(Git revision 1f841cb3044; last commit 2022-03-27)
・Homebrew/homebrew-cask(Git revision 60208d8c20; last commit 2022-03-26)

# PHPのインストール

Homebrewを使用してPHPをインストールします。

まず、HomebrewでインストールできるPHPを探してみます。

```shell
$ brew search php
```

インストール可能なPHPのパッケージが表示されます。今回は現時点でPHP 8系（php@8.0）がリリースされているため、8系を利用する前提で進めます。
インストールするために次のコマンドを実行します。

```shell
# バージョン指定でPHP8をインストール
$ brew install php@8.0
```

PHPのインストールに伴い依存しているパッケージやモジュールが次々にインストールされていくので完了まで待ちます。
インストール完了表示がでますが、このままではパスが通っていないため別途設定が必要となります。

続けてmacOSにPHPの設定をしていきます。


## インストール

## 設定


# Apacheのインストール

## インストール

## セットアップ

# おわりに