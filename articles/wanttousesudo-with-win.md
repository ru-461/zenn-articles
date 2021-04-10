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

# Scoopとは

https://scoop.sh/

Windows 向けのパッケージマネージャーになります。Redhat 系 OS でいう、'yum'・'RPM'、Debian 系 OS だと `apt` にあたります。
コマンドラインから簡単にパッケージをインストールし、依存関係も合わせて適切に管理してくれる便利なものです。Unix 系 OS では一般的に使われるツールですが、Windows にはパッケージ管理の概念があまり浸透していないように感じます。Windows 向けのパッケージマネージャーとして有名なものは[chocolatey](https://chocolatey.org/)、先述した[scoop](https://scoop.sh/)があります。

どちらのツールを使っても Sudo コマンドのインストールが可能です。しかし、Chocolatey を使うためには管理者権限での実行が必要なため、インストールのしやすさだと Scoop が抜き出ています。ただし、インストールできるパッケージの数では Chocolatey のほうが圧倒的に多いので、用途によって使い分けるのが理想かなと感じます。

それでは Scoop を Windows へインストールしていきます。


# おわりに