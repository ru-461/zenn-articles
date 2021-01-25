---
title: "VScode内でNotionを使う[VScode Notion]"
emoji: "💎"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["notion", "vscode"]
published: false
---

# はじめに

私は、普段のメモの管理やデータを「Notion」と呼ばれるオンラインノートを使って一元管理しています。使用例として以前投稿した[こちら](https://zenn.dev/ryuu/articles/8f7513d83f05c77d06a3)の記事内でプログラミングノートの作成について書いていますのでよろしければご覧ください。Notion を自分専用の Wiki のように活用し、一箇所で必要な情報を取得できるようにすることで生産性の向上に繋がります。

今回は VScode 内で Notion を使うことができる VScode Notion という拡張機能を知ったため、導入してみました。

# Notion の課題

私は普段主にプログラミングノートとして学んだことをまとめることや、Zenn で記事を書くときのアイデア出し、タスク管理として使っています。

開発をするとき、記事を執筆するときはローカルのエディタとして Microsoft が開発している Visual Studio Code (VScode)というエディタを愛用しているのですが、Notion を開いて情報を見ながらのコーディングや、記事の執筆をする機会が多いです。そんなときは Notion のアプリやブラウザで Notion を開いて複数のウインドウをまたぎながら作業することになるのですが、この作業が多くなるほどウインドウの切り替えや視線の移動が多くなり大変に感じることが多くありました。

# 「VScode Notion」とは何者か？

VScode に導入して使用できる拡張機能になります。

https://marketplace.visualstudio.com/items?itemName=frenco.vscode-notion

拡張機能はすべて英語で表記されているため、難しそうに思えますが、機能としてはタブとして VScode 内で notion を開くことを可能にする拡張機能となります。
notion ではまだ公式に正式 API が提供されていないため、拡張機能の説明には `unofficial API (非公式 Api)`を使用するとの記述があります。そのため認証機能などは使えず、機能が制限されるとのことですが、今後正式な API の提供に伴い切り替えていくとの記述もあり今後に期待できそうな拡張機能です。

:::message
非公式 API を使用している現在段階では、Notion で作成したページの閲覧だけできる状態となっています。編集をするなどの操作が制限されているため、しばらくは閲覧のみの拡張機能として割り切って使う必要がありそうです。
:::

## VScode Notion の導入

それでは、VScode に拡張機能を導入して Notion を開けるようにしていきます。

拡張機能の検索ウインドウに `notion` といれることで「VSCode Notion」がヒットするのでインストールしていきます。
![](https://storage.googleapis.com/zenn-user-upload/6i6uzl1kglf3dqx7d27trlev3v5m)

インストールが完了するとステータスがインストール済みになり、インストール済みの拡張機能に追加されます。
インストールすることで自動的に拡張機能が有効になり、左のメニューに Notion のアイコンが表示されます。

VSCode Notion を開くと notion の開きたいページの URL を入力するように求められます。
![](https://storage.googleapis.com/zenn-user-upload/xcvqs9sr5jdgobphsr9t3i9tmgdf)

ここに開きたい Notion のページへのアドレスを入れることで開くことができるのですが、デフォルトの状態では `Couldn't load the data from API.` と api エラーが出て開くことができません。

## 初期設定

API エラーで開くことができないため、初期設定が必要となります。拡張機能の設定画面を開きます。VScode 設定画面の検索ウインドウに `vscode notion` と入れることで VSCode Notion の拡張機能の設定を探すことができます。

![](https://storage.googleapis.com/zenn-user-upload/f11qaiyywgrk22clorltljb788n7)

API エラーを解消するためには、下にある `VScode Notion Access Token` のフォームに Notion のアクセストークンを入力する必要があります。

### Notion のアクセストークンの取得

Notion のアクセストークンを取得します。ブラウザにて Notion のページにアクセスし、DeveloperTools を起動します。
`Application` というタブの中に `token_v2`という項目があるため、値をコピーします。これが自分の Notion アクセストークンとなります。

このアクセストークンは拡張機能を使うために必要となるので、慎重に管理をお願いします。

取得した Notion アクセストークンを `VScode Notion Access Token` の項目にそのままペーストします。

アクセストークンが正常に入力されていると `Open A page` から Notion のページリンクを入力することで VSCode 内で指定した Notion のページが開けるようになります。

## ドキュメントの閲覧

以前[こちら](https://zenn.dev/ryuu/articles/8f7513d83f05c77d06a3)の記事で作成したプログラミングノートのトップページを開いてみます。
アドレスで指定したページが VSCode 内で表示できており、問題なく動作しました。

![](https://storage.googleapis.com/zenn-user-upload/oflnx8dsbnzpxr63rj5d4r9do9cp)
_プログラミングノートトップページ - 技術ページへの遷移も可能_

![](https://storage.googleapis.com/zenn-user-upload/ieji7rr5hhhk4bwse6gnbskul1an)
_技術ごとのページ - データベース表示が崩れ気味_

![](https://storage.googleapis.com/zenn-user-upload/ykolgfd6cuzhuhy3tvzogz3ygd1i)
_備忘録メモ - Notion 内のコードブロックもうまく表示できており可読性も問題なし_

## 使ってみて気になった点

トップページをブラウザで見たものが以下になります。

![](https://storage.googleapis.com/zenn-user-upload/vi6278kahmdn2188fjqxuvuwiozy)

テキストは問題なく表示できていますが、オリジナルの Notion と VSCode Notion を比較すると表示に失敗している部分が多々見受けられました。比較してみると。

- フルデータベースページへの直リンク
- ページに設定したアイコンイメージ(ローカルからのアップロードのみ)
- フルデータベースページ

が上手く表示できていないことがわかります。
データベースページヘのリンクを直接開こうとすると `Request failed with status code 404` となり、上手く表示ができませんでした。

# おわりに

VScode で Notion を開くことができるようになることで、Notion で作成したページを見ながらのコーディングが効率化できると感じました。
しかし、非公式 API を使っていることで、機能の制限、不具合なども目立っている印象を受けました。これから公式の API が提供される中で、公式 API に切り替えていくなかでより便利に使えるようになるのを期待したいです。

今後も拡張機能を使っていく中で更新などがありましたら記事の内容も最新の情報に更新していきます。

以上、ここまで読んでいただきありがとうございました。
