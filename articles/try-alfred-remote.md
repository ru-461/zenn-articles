---
title: "Alfred Remote（仮）"
emoji: "📶"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["alfred","alfredremote"]
published: false
---

# はじめに

[Alfred Remote](https://www.alfredapp.com/remote/)というアプリをご存知でしょうか。

Alfredを利用している人なら設定画面に「Remote」という設定項目があることに気づいている人も多いかと感じます。

![Alfredの設定画面](/images/try-alfred-remote/image01.png)

これはその名の通りRemoteでAlfredを操作する設定で、専用のアプリを導入することで有効化てきます。今回はAlfredではなくAlfred Remoteを使ってできることについて紹介します。

# Alfredとは

AlfredとはmacOS向けのランチャーアプリです。ホットキーで簡単に呼び出して、アプリ・ファイルの検索やWebサイトの検索、ワークフロー（手続き処理）などあらゆることを高速に実行できるランチャーアプリです。このAlfredを使いたいからMacを選ぶという人もいるほど根強いフォンを抱えているアプリです。

公式サイトはこちらになります。

https://www.alfredapp.com/

# Alfred Remoteとは

Alfredの紹介で前置きが長くなってしまいましたが、今回紹介するのはAlfredではなく、Alfred Remoteというモバイル向けアプリになります。Alfredの紹介記事はインターネット上に数多く出回っていますが、Alfred Remoteについて説明しているものがあまり見られません。App Storeのレビューも近年のものがあまりなく動作するのかもわからず、導入を躊躇していました。

今回そんなAlfred Remoteを導入してどこまでの操作ができるか、作業を効率化できるかをやってみました。

Alfred Remoteは有料アプリになります。現状対応しているのがiPhone、iPadのみとなり、App Storeにて購入できます。私が購入したときは610円でした。以下のストアページになります。

https://apps.apple.com/jp/app/alfred-remote/id927944141

:::message
海外のアプリのため時期によっては価格が変更になる可能性があります。
:::

# Alfred Remoteを使う上での前提条件

上記リンクAppStoreからインストールします。Alfred RemoteはiOS・iPadOSのみのサポートになります。Androidには対応していないので注意してください。

またAlfred Remoteのアプリ単体では使用できないため、Mac側にAlfredがインストールされていること、有料コンテンツである[Power Pack](https://www.alfredapp.com/powerpack/)が導入されていることが必須になります。

AlfredにPower packを導入することでワークフロー機能がアンロック、コピー履歴保存機能、カスタムテーマなどが使えるようになります。Power Packにはバージョンごとの買い切りライセンスとライフタイムライセンス（永久ライセンス）の2種類が存在します。違いはバージョンアップの際のサポートが受けられるか受けられないかの違いと価格です。ライフタイムライセンスはバージョンライセンスに比べて倍近くの価格がしますが、1回の購入でずっとサポートを受けられるのが大きいです。私は、Alfredを使わない日がないくらいに使い込んでいるので迷わずライフタイムライセンスを購入しました。

:::message
Power Packの価格は単位がポンド表記のため時期によって価格が変動します。購入前によく調べて日本円での相場を確認することをおすすめします。
:::

# Alfred Remoteのセットアップ

## デバイス登録

Alfred RemoteをインストールしたらホストマシンとなるMacとの登録作業が必要です。Alfred Remoteのインストールが終わるとMacとの接続画面になります。アプリ自体に設定はなく、全てホスとマシンであるMac上にておこないます。

Alfredが使用できるMacとAlfred Remoteがインストールされたデバイスを同じネットワークに接続します。次にAlfredの設定のRemoteから「add iOS remote」をクリックすることで接続準備が整います。あとは画面に表示されたパスフレーズを入力して認証することですぐに繋がります。1回デバイスを認証すれば、次からはAlfred Remoteのアプリを開くだけで自動的に接続してくれます。

# おわりに