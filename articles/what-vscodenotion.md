---
title: "VSCodeでNotionを使う【VSCode Notion】"
emoji: "💎"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["notion", "vscode", "ツール"]
published: true
---

# はじめに

私は普段のメモの管理やタスクを「Notion」と呼ばれるオンラインノートアプリを使って一元管理しています。
Notionを自分専用のWikiのように活用し、一箇所で必要な情報を取得できるようにすることで生産性の向上に繋がります。

以前投稿した[こちらの記事](https://zenn.dev/ryuu/articles/8f7513d83f05c77d06a3)でNotionをプログラミングノートとして運用する方法について書いていますのでよろしければ合わせてご覧ください。

今回はVSCode内でNotionを使うことができる`VSCode Notion`という便利な拡張機能をご紹介します。

# Notion の課題点

私は普段主にプログラミングノートとして学んだことをまとめることや、Zennで記事を書くときのアイデア出し、タスク管理として使っています。

私は、ローカルのエディタとしてMicrosoftが開発しているVisual Studio Code（VSCode）というエディタを愛用しています。隣には、いつもNotionを開いて情報を見ながらのコーディングや、記事の執筆をする機会が多いです。これだと2つのアプリを行き来することになるので、作業が多くなるほどウィンドウの切り替えや視線の移動が多くなり大変に感じることが多くありました。

# 「VSCode Notion」とは何者か？

VSCodeに導入して使用できる拡張機能になります。

https://marketplace.visualstudio.com/items?itemName=frenco.VScode-notion

拡張機能はすべて英語で表記されているため、難しそうに思えますが、機能としてはタブとしてVSCode内でNotionを開くことを可能にする拡張機能となります。
更新の履歴を見てみると2021年の1月10日に初期バージョンがリリースされたばかりの拡張機能であることがわかります。

拡張機能の説明には`unofficial API（非公式 API）`を使用するとの記述があります。そのため認証機能などは使えず、いくつかの機能が制限されるとのことですが、今後正式なAPIの提供に伴い切り替えていくとの記述があり今後に期待できそうな拡張機能です。先日、Notion APIのPublicBetaが公開されたため今後に注目したいですね。

:::message
非公式APIを使用している現在段階では、Notionで作成したページの閲覧だけできる状態となっています。編集をするなどの操作が制限されているため、しばらくはNotionドキュメントの閲覧だけに使えるものと割り切って使う必要がありそうです。
:::

## VSCode Notion の導入

それでは、VSCodeに拡張機能を導入してNotionを開けるようにしていきます。

拡張機能の検索ウィンドウに`notion`といれることで「VSCode Notion」がヒットするのでインストールしていきます。

![VSCode Notionを検索する様子](https://storage.googleapis.com/zenn-user-upload/6i6uzl1kglf3dqx7d27trlev3v5m)
*検索してヒットしたこちらの拡張機能をインストール*

インストールが完了するとステータスがインストール済みになり、インストール済みの拡張機能に追加されます。
インストールすることで自動的に拡張機能が有効になり、左のメニューにNotionのアイコンが表示されます。

VSCode Notionを開くと`RECENTS（最近VSCodeで開いたページ）`と`BOOKMARKS（ブックマークしたページ）`の2つから選ぶことができます。

初めて使用するときはまだページを開いていないため、 Notionページを開くための`Open A Page`ボタンが表示されています。

![Notionのページを開く様子](https://storage.googleapis.com/zenn-user-upload/xcvqs9sr5jdgobphsr9t3i9tmgdf)
*Open A PageからNotionのページを開くことができる*

Notionのページリンクを入力して指定したページを開くことができるのですが、デフォルトでは`Couldn't load the data from API.`とAPIのエラーが出て開くことができません。
Notionのページを開くためには**アクセストークンを取得して設定**する必要があります。
デフォルトの状態ではアクセストークンが空になっているため以下の初期設定が必要となります。

## 初期設定

VSCodeの設定画面を開きます。設定画面はWindowsであれば`ctrl + ,` Macなら`⌘ + ,`で開くことができます。VSCode設定画面の検索ウィンドウに`vscode notion`と入れることでVSCode Notionの拡張機能の設定を探すことができます。設定が検索にヒットしない場合はVSCodeを一旦再起動することで表示されるようになります。

![設定を検索する様子](https://storage.googleapis.com/zenn-user-upload/f11qaiyywgrk22clorltljb788n7)

APIエラーを解消するためには、下にある`VSCode Notion Access Token`のフォームに取得したNotionのアクセストークンを入力する必要があります。

## Notion のアクセストークンの取得

Notionのアクセストークンを取得します。ブラウザにてNotionのページにアクセスし、DevToolsを起動します。

以下Chromeの画面で説明します。

`Application`というタブを開き、`Cookies > https://www.notion.so/`とたどると`token_v2`という項目があるため、値をコピーします。
これが自分のNotionアクセストークンとなります。

![Chrome DevToolsからトークンを取得する様子](https://storage.googleapis.com/zenn-user-upload/vg3rhxbtu5tnwg1582frn4iss0q1)
*valueの項目にある値をそのままコピー*

アクセストークンは長い文字列になっており、拡張機能を使うために必ず必要となるので、慎重に管理をお願いします。

取得したNotionアクセストークンを設定画面の`VSCode Notion Access Token`にそのままペーストします。

![取得したトークンを設定する様子](https://storage.googleapis.com/zenn-user-upload/g6yakw9g75hx8eh6zjssg38fu5q2)
*赤枠の中にそのままペースト*

これで初期設定が完了となります。

拡張機能の設定にアクセストークンが正常に入力されていると`Open A Page`からNotionを開けるようになります。ページリンクを入力することで指定したNotionのページがVSCode内で開けます。

## ドキュメントの閲覧

以前[こちら](https://zenn.dev/ryuu/articles/8f7513d83f05c77d06a3)の記事で作成したプログラミングノートのトップページを開いてみます。
アドレスで指定したページがVSCode内で表示できており、問題なく動作しました。

![VSCodeから見たプログラミングノートトップページの画像](https://storage.googleapis.com/zenn-user-upload/oflnx8dsbnzpxr63rj5d4r9do9cp)
*プログラミングノートトップページ - 技術ページへの遷移も可能*

![VSCodeから見た技術個別ページの画像](https://storage.googleapis.com/zenn-user-upload/ieji7rr5hhhk4bwse6gnbskul1an)
*技術ごとのページ - データベース表示が崩れ気味*

![VSCodeから見た備忘録メモページの画像](https://storage.googleapis.com/zenn-user-upload/ykolgfd6cuzhuhy3tvzogz3ygd1i)
*備忘録メモ - Notion 内のコードブロックもうまく表示できており可読性も問題なし*

## 使ってみて気になった点

トップページを**ブラウザ**で見たものが以下になります。

![ブラウザから見たプログラミングノートの画像](https://storage.googleapis.com/zenn-user-upload/vi6278kahmdn2188fjqxuvuwiozy)
*ブラウザで見たときの表示*
テキストは問題なく表示できていますが、オリジナルのNotionとVSCode Notionを比較すると表示に失敗している部分が多々見受けられました。比較してみると。

- フルデータベースページへの直リンク
- ページに設定したアイコンイメージ（ローカルからのアップロードのみ）
- フルデータベースページ

が上手く表示できていないことがわかります。
データベースページヘのリンクを直接開こうとすると`Request failed with status code 404`となり、上手く表示ができませんでした。

# おわりに

VSCodeでNotionを開くことができるようになることで、Notionで作成したページを見ながらのコーディングが効率化できると感じました。
しかし、非公式APIを使っていることでいくつかの機能が制限されていたり、不具合なども目立っている印象を受けました。これから公式のAPIが提供される中でより便利になっていくものと思われます。

まだリリースされてまもなく、開発が盛んに行われているため、今後どんどんアップデートされることを期待しましょう。

最後まで読んでいただきありがとうございました。