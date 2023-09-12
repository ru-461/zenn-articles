---
title: "【macOS Monterey】HomebrewでPHPの実行環境をセットアップする"
emoji: "🐘"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["mac", "php", "homebrew"]
published: true
---

## はじめに

macOS 12ことMontereyがリリースされてしばらく日が経ちました。ユニバーサルコントロール対応などの目玉機能が注目されるなか、密かにPHPがmacOSにバンドルされなくなっていることに気づきました。
いままでのmacOSではインストール時からPHPがセットアップされて使える状態になっているというのが暗黙の了解で常識化していたように感じます。

macOS 12移行にアップグレードした環境で`php --version`をすると、`command not found: php`と言われてしまいます。どうやらPHPは含まれなくなったようです。
新規にmacOS Montereyをインストールした際はもちろんのこと、アップグレードした場合でも削除されるようです。アップグレード後にきれいさっぱりPHPがいなくなりました。

そこでmacOSでパッケージマネージャHomebrewを使用してPHPの実行環境をセットアップするまでにやったことまとめてみます。恥ずかしながらかなりハマったポイントがあるためこれから新規にPHPをmacOSにセットアップする方の参考になれば嬉しいです。

PHPの公式サイトでも一番手っ取り早いインストール方法として紹介されているものになります。

https://www.php.net/manual/ja/install.macosx.packages.php

## 使用環境

・Macbook Pro(13-inchi, M1, 2020)
・macOS Monterey(バージョン 12.3)
・Homebrew/homebrew-core(Git revision 1f841cb3044; last commit 2022-03-27)
・Homebrew/homebrew-cask(Git revision 60208d8c20; last commit 2022-03-26)

## PHPのインストール

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

### セットアップ

インストールしたPHPは自動的にパスが通らないため、シェルのプロファイルからパスを手動で通す必要があります。使用しているシェルのプロファイル（.bascrcや.zshrc）などを開き以下コマンドを叩くか、プロファイルを直接編集しパスを構成します。Zshを使用している場合は以下のようになります。

```shell
$ echo 'export PATH="/opt/homebrew/opt/php@8.0/bin:$PATH"' >> ~/.zshrc
$ echo 'export PATH="/opt/homebrew/opt/php@8.0/sbin:$PATH"' >> ~/.zshrc
```

コマンドを実行後、パスを読み込ませるために一旦シェルを再起動します。シェルを再起動したあとに`php --version`でPHPのバージョン情報が出力されれば問題なくパスが通り使用できるようになっています。

パスが通ることを確認できたら併せて以下の設定も行います。シェルのプロファイル内に次の２行を追記します。

```shell:.zshrc
export LDFLAGS="-L/opt/homebrew/opt/php@8.0/lib"
export CPPFLAGS="-I/opt/homebrew/opt/php@8.0/include"
```

こちらも追記後にシェルを再読み込みし有効化しておきます。

これでPHPを使用できる環境が構築できました。

## おわりに

PHPをmacOS 12以降で利用できるようにする方法についてまとめました。Homebrewで構築する場合依存関係を解決し適切にパスを通すだけで使えるようになるのはやはり便利です。
いままで依存関係でPHPのビルドに苦戦することが多かったので、Homebrewの快適さに驚きました。

同じようにmacOS 12以降でPHPを必要とする方の参考になれば幸いです。

最後まで読んでいただきありがとうございました。
