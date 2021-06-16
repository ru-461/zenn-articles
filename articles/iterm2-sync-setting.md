---
title: "iTerm2 の設定を Dropbox で共有して同期する"
emoji: "📁"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["iterm2", "dropbox"]
published: true
---

# はじめに

> [iTerm2](https://iterm2.com/) 使ってますか？

iTerm2 とは macOS で使用できるターミナルアプリです。エンジニア界隈では、かなり有名なので現在使用している人でも多いのではないしょうか。

iTerm2 はカスタマイズ機能にも優れており、自分の使いやすいターミナル環境を作ることができます。また、いつも使うツールになるので設定を常にバックアップしてデータを管理し、いつ環境が変わっても同じ設定を使いたいことが往々にしてあります。

今回、iTerm2 とクラウドストレージサービスの [Dropbox](https://www.dropbox.com/ja/) を使用し、設定を管理、同期する方法について紹介します。

# 設定ファイルの保存先をDropboxにする

iTerm2 を起動して、`設定(Preferences)` を開きます。メニューから `iTerm2 → Preferences` またはショートカットキーで `⌘ + ,` とすることで開くことができます。

設定ウインドウが開いたら `Generalタブ` から `Preferences` を選択します。

![設定ファイル保存場所設定の画像](https://storage.googleapis.com/zenn-user-upload/c18c98bb50725c47100a6202.png)
*設定ファイルの保存先選択*

iTerm2 は設定ファイルの保存先をカスタムでき、自由に保存先を指定できます。

Dropbox のアプリを Mac にインストールして使っている場合、Finder から Dropbox へアクセス可能となり、Dropbox 上の場所も指定できるようになります。

https://help.dropbox.com/ja-jp/installs-integrations/desktop/locate-dropbox-folder

[Dropbox を公式サイトからインストール](https://www.dropbox.com/install)し、セットアップを進めることで Dropbox ディレクトリにアクセス可能となります。

```shell
# ユーザーディレクトリからDropboxへ移動
$ cd ~/Dropbox
```

上のようにすることで Dropbox へアクセスできます。Dropbox を開けることを確認したら iTerm2 の設定ファイルの保存場所を作成します。

```shell
# Dropbox ディレクトリの直下に Mac / Settingsディレクトリを作成(任意)
~/Dropbox  $ mkdir -p Mac/Settings
```

Dropbox 上で保存する場所はどこでも大丈夫です。私は他のアプリの設定も合わせて保存する場所として以下のようなディレクトリ構成にしています。

```shell
# 上のコマンドでできる構成
Dropbox(.)
└── Mac
   └── Settings
```
ディレクトリを作成したら iTerm2 の設定ファイルの保存先を指定します。

iTerm2 の Preference から `Load preferences from a custom folder or URL` にチェックを入れると Finder が開くので、先程 Dropbbox に作成したファイルの保存場所を選択しセットします。

ディレクトリのセットが完了しパスの横に ✅ の表示がでれば保存先の設定は完了です。
いますぐ同期をとる場合は、`Sync Now` をクリックすることですぐに設定ファイルの同期が行われます。

SaveChanges のメニューから保存するタイミング、頻度を設定できます。デフォルトで `Automatically` となっていますが、自動的にで保存しておいてもらいたいのでそのまま(Automatically)にしています。

これで常に iTerm2 の設定を Dropbox 上で管理できるようになりました。新しい環境に移行したときも Dropbbox を Mac へインストールし、設定ファイルの場所を選択することで自分の設定を反映させることができます。

# おわりに

今回、Mac ユーザーにおなじみのターミナルアプリ iTerm2 にて設定を同期させる設定について書きました。iTerm2 は設定項目が全部英語ということもあり、なかなか気づかないけど使えるとかなり便利な項目が多い印象を受けました。便利なツール、いつも使うツールは自分の使いやすいようにカスタマイズすることで生産性を上げることにも繋がります。カスタマイズした環境を同期できるのはとても便利なので、ぜひ設定自分にあった設定を探してみてください。

最後まで読んでいただきありがとうございました。