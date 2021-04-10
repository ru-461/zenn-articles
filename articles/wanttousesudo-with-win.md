---
title: "WindowsにもおなじみのSudoコマンドを導入する"
emoji: "⚡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["windows","powershell","scoop"]
published: false
---

# はじめに

Unix 系の OS では管理者権限で実行する際に `sudo コマンド` を主に使用します。ログアウトせずに一時的に管理者権限(root)に切り替えてコマンドを実行できることもあり、Linux の環境構築などで多用されます。

そんな便利な ` sudo コマンド` ですが、Windows を使う際にも使えたらなと思ったことはないでしょうか。

Windows には、コマンドプロンプトや Powershell を起動する際「管理者として実行する」という選択肢が存在し、管理者権限でコマンドを叩くことが可能です。ですが、すべてのコマンドを管理者権限で実行する状態になるため、Linux と比較したときに権限周りの柔軟性で欠ける部分があります。

そこで `Scoop` という Windows 向けパッケージマネージャーを使用し、 Sudo コマンドをインストール、使用できるようになるまでを紹介します。

# Scoopとは

Windows 向けのパッケージマネージャーになります。

https://scoop.sh/

有名なものとして RedHat 系 OS でいう、`yum`や `RPM` 、Debian 系 OS だと `apt` 、MacOS での `homebrew` にあたります。

どの環境においてもパッケージマネージャーはコマンドラインから簡単にパッケージをインストールし、依存関係も合わせて適切に管理するという役割を持ち欠かすことのできないものになっています。Unix 系 OS(Linux や MacOS) ではプログラミングのチュートリアルや環境構築ドキュメント内で一般的に使われるツールですが、Windows だと個別にインストーラーを使う方法が一般的でパッケージ管理マネージャーの概念があまり浸透していないように感じます。

そこで Windows でもパッケージ管理を行えるツールとして有名なものに[Chocolatey](https://chocolatey.org/)、先述した[Scoop](https://scoop.sh/)があります。

どちらのツールを使っても Sudo コマンドを始めとする様々なコマンドのインストールが可能です。しかし、Chocolatey のインストールには管理者権限が必要なため、インストールのしやすさだと Scoop が抜き出ています。ただし、インストールできるパッケージの数では Chocolatey のほうが圧倒的に多いので、用途や環境によって使い分けるのが理想かなと感じます。

それでは Scoop を Windows へインストールしていきます。

## Scoopのインストール

インストールはとても簡単で、PowerShell を起動して以下のスクリプトを実行するだけで導入できます。

```shell:Powershell
$ iwr -useb get.scoop.sh | iex
```
うまく実行できないときは以下のコマンドでユーザーポリシーを緩めることでインストールできます。
```shell:Powershell
$ Set-ExecutionPolicy RemoteSigned -scope CurrentUser
```
Scoop コマンドが実行できることを確認できればインストールは完了です。
```shell:Powershell
$ scoop

Usage: scoop <command> [<args>]
...
```
ヘルプが表示されます。


# Sudoコマンドのインストール

先程インストールした Scoop を使用して Sudo コマンドをインストールしていきます。
```powershell
$ scoop install sudo

Installing 'sudo' (0.2020.01.26) [64bit]
sudo.ps1 (2.2 KB) [======/ ~ ======] 100%
Checking hash of sudo.ps1 ... ok.
Linking ~\scoop\apps\sudo\current => ~\scoop\apps\sudo\0.2020.01.26
Creating shim for 'sudo'.
'sudo' (0.2020.01.26) was installed successfully!
```
`installed successfully!`となれば Sudo コマンドのインストール成功です。

:::message
Scoop を使ってインストールしたコマンドの実体は~\scoop\apps\shims に存在します。Scoop 経由でインストールしたものはすべてユーザーディレクトリの決まった場所にまとめられるため、インストールと実行に管理者権限が必要ありません。
:::

# 実行してみる

インストール直後に自動でパスが通っているため、コマンドプロンプトや PowerShell からおなじみの Sudo コマンドを実行できます。

```
# 確認
$ sudo

usage: sudo <cmd...>
```

例として、編集に管理者権限が必要な hosts ファイルを管理者権限でメモ帳を使って開いてみます。
```
# notepad(メモ帳)でhostsファイルを指定
$ sudo notepad C:\Windows\System32\drivers\etc\hosts
```

管理者に昇格するための UAC(ユーザーアカウント制御) が表示されます。

![ユーザーアカウント制御の画面](https://storage.googleapis.com/zenn-user-upload/5gbhypvzvgkalw6r3xuzrubs8vkr)

続行すると管理者権限で hosts ファイルをメモ帳で開くことができます。

![管理者権限でhostsファイルを開いた様子](https://storage.googleapis.com/zenn-user-upload/6qc6m9myaywrqv6v70onc5zwwaj2)

このように Sudo コマンドをインストールすることで Unix 系 OS と同じ感覚で一時的に管理者に昇格してコマンド・アプリの実行ができるようになります。

# おわりに

Scoop を使って Windows で Sudo コマンドを使えるようにする方法について紹介しました。Windows でも一時的に管理者権限で操作したい処理があるときに手軽に管理者権限に切り替えて実行できるのは便利です。また紹介した Scoop というパッケージマネージャーを使うことで様々なコマンドを Windows にも導入できます。Scoop を活用することで開発環境の構築、コマンドラインを使った Windows の操作が楽になるため、使えるものはどんどん活用していきたいところです。

最後まで読んでいただきありがとうございました。

# 参考

- [sudo - Wikipedia](https://ja.wikipedia.org/wiki/Sudo)
- [Luke Sampson-Sudo for Windows](http://blog.lukesampson.com/sudo-for-windows)
- [全Scoopコマンド解説 その１ ～使用頻度（高）～ - Qiita](https://qiita.com/nimzo6689/items/1ab33380366e324c0b84)