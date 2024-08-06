---
title: "Windowsにおなじみのsudoコマンドを導入する"
emoji: "⚡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["windows", "powershell", "scoop"]
published: true
---

## はじめに

UNIX系のOSでは管理者権限で実行する際に`sudo`コマンドを使用します。ログアウトせずに一時的に管理者権限に切り替えてコマンドを実行できることもあり、Linuxの環境構築などで多用されます。

そんな便利な`sudo`ですが、Windowsを使う際にも使えたらなと思ったことはないでしょうか。

Windowsには、コマンドプロンプトやPowershellを起動する際「管理者として実行する」という選択肢が存在し、管理者権限でコマンドを叩くことが可能です。ですが、すべてのコマンドを管理者権限で実行する状態になるため、Linuxと比較したときに権限周りの柔軟性で欠ける部分があります。

そこでScoopというWindows向けパッケージマネージャーを使用し、 sudoコマンドをインストール、使用できるようになるまでを紹介します。

## Scoop とは

結論から述べると、**Windows向けのパッケージマネージャー**になります。
https://scoop.sh

有名なものとしてRed Hat系OSでいう、yumやRPM、Debian系OSだとapt、macOSでのHomebrewにあたります。

どの環境においてもパッケージマネージャーは欠かすことのできないものになっています。UNIX系OS（LinuxやmacOS）では一般的に使われるツールですが、Windowsだとインストーラーが一般的でパッケージ管理マネージャーの概念があまり浸透していないように感じます。

そこでWindowsでもパッケージ管理を行えるツールとして有名なものに[Chocolatey](https://chocolatey.org)、先述した[Scoop](https://scoop.sh)があります。

どちらのツールを使ってもsudoコマンドを始めとする様々なコマンドのインストールが可能です。しかし、Chocolateyのインストールには管理者権限が必要なため、インストールのしやすさだとScoopが抜き出ています。ただし、インストールできるパッケージの数ではChocolateyのほうが圧倒的に多いので、用途や環境によって使い分けるのが理想かなと感じます。

それではScoopをWindowsへインストールしていきます。

### Scoop のインストール

インストールはとても簡単で、PowerShellを起動して以下のスクリプトを実行するだけで導入できます。

```powershell:powershell
> iwr -useb get.scoop.sh | iex
```

うまく実行できないときは以下のコマンドでユーザーポリシーを緩めることでインストールできます。

```powershell:powershell
> Set-ExecutionPolicy RemoteSigned -scope CurrentUser
```

Scoopコマンドが実行できることを確認できればインストールは完了です。

```powershell:powershell
> scoop

Usage: scoop <command> [<args>]
...
```

## sudoコマンドのインストール

先程インストールしたScoopを使用してsudoコマンドをインストールしていきます。

```powershell:powershell
# Scoop経由でsudoコマンドをインストールする
> scoop install sudo

Installing 'sudo' (0.2020.01.26) [64bit]
sudo.ps1 (2.2 KB) [======/ ~ ======] 100%
Checking hash of sudo.ps1 ... ok.
Linking ~\scoop\apps\sudo\current => ~\scoop\apps\sudo\0.2020.01.26
Creating shim for 'sudo'.
'sudo' (0.2020.01.26) was installed successfully!
```

`installed successfully!`となればsudoコマンドのインストール成功です。

:::message
Scoopを使ってインストールしたコマンドの実体は~\scoop\apps\shimsに存在します。Scoop経由でインストールしたものはすべてユーザーディレクトリの決まった場所にまとめられるため、インストールと実行に管理者権限が必要ありません。
:::

## 実行してみる

インストール直後に自動でパスが通っているため、コマンドプロンプトやPowerShellからおなじみのsudoコマンドを実行できます。

```powershell:powershell
# 確認
> sudo

usage: sudo <cmd...>
```

例として、編集に管理者権限が必要なhostsを管理者権限でメモ帳を使って開いてみます。

```powershell:powershell
# notepad(メモ帳)でhostsを指定
> sudo notepad C:\Windows\System32\drivers\etc\hosts
```

管理者に昇格するためのUAC（ユーザーアカウント制御）が表示されます。

![ユーザーアカウント制御の画面](/images/wanttousesudo-with-win/image01.png)

続行すると管理者権限でhostsをメモ帳で開くことができます。

![管理者権限でhostsを開いた様子](/images/wanttousesudo-with-win/image02.png)

このようにsudoコマンドをインストールすることでUNIX系OSと同じ感覚で一時的に管理者に昇格してコマンド・アプリの実行ができるようになります。

## おわりに

Scoopを使ってWindowsで`sudo`を使えるようにする方法について紹介しました。Windowsでも一時的に管理者権限で操作したい処理があるときに手軽に管理者権限に切り替えて実行できるのは便利です。また紹介したScoopというパッケージマネージャーを使うことで様々なコマンドをWindowsにも導入できます。Scoopを活用することで開発環境の構築、コマンドラインを使ったWindowsの操作が楽になるため、使えるものはどんどん活用していきたいところです。

最後まで読んでいただきありがとうございました。

## 参考

- [sudo - Wikipedia](https://ja.wikipedia.org/wiki/Sudo)
- [Luke Sampson-Sudo for Windows](http://blog.lukesampson.com/sudo-for-windows)
- [全Scoopコマンド解説 その１ ～使用頻度（高）～ - Qiita](https://qiita.com/nimzo6689/items/1ab33380366e324c0b84)
