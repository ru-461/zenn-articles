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

# おわりに