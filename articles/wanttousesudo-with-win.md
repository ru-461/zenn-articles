---
title: "WindowsでもSudoを使いたい"
emoji: "⚡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["windows","powershell"]
published: false
---

# はじめに

Unix 系の OS では管理者権限で実行する際に `sudo コマンド` を主に使用します。ログアウトせずに一時的に管理者権限(root)にてコマンドを実行できることもあり、Linux の環境構築などで多用されます。
そんな便利な ` sudo コマンド` ですが、Windows を使う際にも使えたらなと思ったことはないでしょうか。

Windows には、コマンドプロンプトや Powershell を起動する際に(Run As Admin)管理者として実行するという選択肢が存在し、管理者権限でコマンドを叩くことが可能です。ですか、Linux のように一般権限から一時的に管理者言言に切り替えるなどが難しい状況であり、Linux と比較したときに柔軟性で欠ける部分があります。

そこで Scoop という Windows 向けパッケージマネージャーを使用し、 Sudo コマンドをインストール、使用できるようになるまでを紹介します。

# おわりに