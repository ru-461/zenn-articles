---
title: "iTerm2の設定をDropboxで共有して同期する"
emoji: "📁"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["iterm2", "dropbox"]
published: true
---

## はじめに

「突然ですが [iTerm2](https://iterm2.com)使ってますか」

iTerm2とはmacOSで使用できるターミナルアプリです。エンジニア界隈では、かなり有名なので現在使用している人でも多いのではないしょうか。

iTerm2はカスタマイズ機能にも優れており、自分の使いやすいターミナル環境を作ることができます。また、いつも使うツールになるので設定を常にバックアップしてデータを管理し、いつ環境が変わっても同じ設定を使いたいことが往々にしてあります。

今回、iTerm2とクラウドストレージサービスの[Dropbox](https://www.dropbox.com/ja)を使用して設定を管理、同期する方法について紹介します。

## 設定ファイルの保存先をDropboxにする

iTerm2を起動して、設定（Preferences）を開きます。メニューからiTerm2 → Preferencesまたはショートカットキーで`⌘`＋`,`とすることで開くことができます。

設定ウィンドウが開いたらGeneralタブからPreferencesを選択します。

![設定ファイル保存場所設定の画像](/images/iterm2-sync-setting/image01.png)
*設定ファイルの保存先選択*

iTerm2は設定ファイルの保存先をカスタムでき、自由に保存先を指定できます。

DropboxのアプリをMacにインストールして使っている場合、FinderからDropboxへアクセス可能となり、Dropbox上の場所も指定できるようになります。
https://help.dropbox.com/ja-jp/installs-integrations/desktop/locate-dropbox-folder

[Dropbox を公式サイトからインストール](https://www.dropbox.com/install)し、セットアップを進めることでDropboxディレクトリにアクセス可能となります。

```shell
$ cd ~/Dropbox
```

Dropboxディレクトリを開けることを確認したらiTerm2の設定ファイルの保存場所を作成します。

```shell
# Dropboxディレクトリの直下に Mac / Settings ディレクトリを作成(任意)
~/Dropbox $ mkdir -p Mac/Settings
```

Dropbox上で保存する場所はどこでも大丈夫です。私は他のアプリの設定も合わせて保存する場所として以下のようなディレクトリ構成にしています。

```shell
# 上のコマンドでできる構成
Dropbox(.)
└── Mac
   └── Settings
```

ディレクトリを作成したらiTerm2の設定ファイルの保存先を指定します。

Preferenceから"Load preferences from a custom folder or URL"にチェックを入れるとFinderが開くので、先程作成したファイルの場所を選択します。

ディレクトリのセットが完了しパスの横に✅の表示がでれば保存先の設定は完了です。
いますぐ同期をとる場合は、Sync Nowをクリックすることですぐに設定ファイルの同期が行われます。

SaveChangesのメニューから保存するタイミング、頻度を設定できます。デフォルトでAutomaticallyとなっていますが、自動的で保存しておいてもらいたいのでそのままにしています。

これで常にiTerm2の設定をDropbox上で管理できるようになりました。新しい環境に移行したときもDropbboxをMacへインストールし、設定ファイルの場所を選択することで自分の設定を反映させることができます。

## おわりに

今回、MacユーザーにおなじみのターミナルアプリiTerm2にて設定を同期させる設定について書きました。iTerm2は設定項目が全部英語ということもあり、なかなか気づかないけど使えるとかなり便利な項目が多い印象を受けました。**便利なツール、いつも使うツールは自分の使いやすいようにカスタマイズすることで生産性を上げることにも繋がります**。カスタマイズした環境を同期できるのはとても便利なので、ぜひ設定自分にあった設定を探してみてください。

最後まで読んでいただきありがとうございました。
