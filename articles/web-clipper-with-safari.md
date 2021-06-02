---
title: "SafariがNotion Web Clipperに対応したので使ってみる"
emoji: "🧭"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["notion","safari","拡張機能"]
published: true
---

# はじめに

Notion には公式の拡張機能として[Notion Web Clipper](https://www.notion.so/web-clipper)というものがあります。

![Notion Web Clipperのトップページ画像](https://storage.googleapis.com/zenn-user-upload/t6s7p5g4s5lnm8sgzu9u3wcvyaey)

Web Clipper という名の通り、ブラウザで今見ている Web ページを Notion のページに変換し保存できるすぐれものです。Chrome や Firefox では[拡張機能](https://chrome.google.com/webstore/detail/notion-web-clipper/knheggckgoiihginacbkhaalnibhilkk)・[アドオン](https://addons.mozilla.org/ja/firefox/addon/notion-web-clipper/)として前から提供されていましたが、Safari では使用できない状態となっていました。モバイル版 Safari（iPhone・iPad）からは共有機能を使って Notion を開くことで Web Clipper が使用できていましたが、PC 版の Safari には対応していませんでした。

私は、基本的に Chrome で作業することが多かったため、Mac の Safari をあまり使うことがなかったのですが、先日、Notion Web Clipper が Safari に対応したと聞き、実際に Safari へ Notion Web Clipper を導入ししばらく使ってみました。

https://twitter.com/NotionHQ/status/1387825941263486976

Notion Web Clipper は Notion を使うならぜひとも合わせて使いたいくらいに便利なものなのでご紹介します。

# Safariに拡張機能を導入する

あまり知られていないように感じますが、Safari も Chrome や Firefox と同様に拡張機能を導入することで、さらに機能を増やすことができます。Safari へ拡張機能を追加する際は Mac の AppStore から拡張機能を入手しインストールする形となります。

## 導入済みの拡張機能を管理する

Safari の設定画面を開き、拡張機能の項目を開くことで現在 Safari に導入している拡張機能を確認できます。

![拡張機能の管理画面画像](https://storage.googleapis.com/zenn-user-upload/l48blskbtvtfv2e18ss0psab2xhc)
*導入しているアプリによって拡張機能が入っている場合もあります*

ここに拡張機能を検索して追加することで管理が可能となります。チェックマークの付け外しで有効無効の切り替えを行ったり、アンインストールもここから行うことができます。

また右下には `拡張機能を追加...` というボタンがあり、ここから AppStore の Safari 拡張機能カテゴリページを開くことができます。
## Mac AppStoreで拡張機能を探す

Mac AppStore のサイドメニューからカテゴリを選択すると一覧の中に `Safari拡張機能` があります。

![拡張機能カテゴリの画像](https://storage.googleapis.com/zenn-user-upload/2qx25c8y1h1grxzpp70dw4c5qnzm)

ここに Safari で使用可能な拡張機能がまとまっています。無料のものから有料のものまで数多くありますが、ランキング形式で見やすくまとめられているのが探しやすくて便利です。Safari の拡張機能は、通常の Mac アプリと同じようにインストールが可能で、デフォルトでアプリケーションフォルダ(~/Applications)の中に格納されます。

# Notion Web ClipperをSafariにインストール

それでは、Safari に Notion Web Clipper をインストールしていきます。
Mac AppStore 内の検索から「Notion」と検索することで上位にヒットします。

![Notion Web Clipperの画像](https://storage.googleapis.com/zenn-user-upload/zt8r2t3v6grx0gl8irfhf358q4gj)
*インストール済みの場合は「開く」に変わる*

または以下のリンクから直接、ダウンロードページを開くことができます。

https://apps.apple.com/jp/app/notion-web-clipper/id1559269364?mt=12

拡張機能の詳細ページを開いたら通常のアプリと同じように `インストール` をクリックして導入します。サインインが求められた場合は、パスワードまたは生体認証にてサインインすることでインストールが始まります。

インストールした後は Safari の設定から拡張機能を開くことで有効 / 無効を切り替えられるようになります。インストールしてもうまく反映されない場合は拡張機能が有効になっているかどうかを見直してみてください。

# Notion Web ClipperでWebページをクリップする

ここからは Chrome や Firefox と共通の操作になります。

拡張機能を導入すると Safari の上部メニューに Notion のアイコンが表示されるようになり、ここから Notion Web Clipper にアクセスできます。

初回起動時は Notion のログインページへ促され Notion のアカウントでログインする画面になります。ログイン後、Web ページを開いた状態で Notion Web Clipper を起動すると以下のような表示になります。

![Webページをクリップする様子](https://storage.googleapis.com/zenn-user-upload/77eyplq3pgk5xp8o0j3augleiz23)
*Notion Web Clipper のページをクリップする様子*

Notion にログインしてあるので、Workspace と保存先のページを選ぶ項目が表示されています。

上の例だと `MySpace` という Workspace 内にある `WebClips` というデータベースに保存するという意味になります。Web ページを保存する用のデータベースページを予め作成しておくのがおすすめです。この状態で `Save page` ボタンをクリックすることで瞬時に指定したデータベースへ Web ページが保存されます。

保存したあとに表示される `Open in Notion` をクリックすることでクリップしたページをすぐに Notion で開いて確認できるのが便利です。

# おわりに

今回、Safari の拡張機能と Notion Web Clipper を導入する方法について書きました。初めて Safari に拡張機能を導入しましたが、Safari も Chrome と同じように拡張機能を導入できるのが便利だと感じました。有名な拡張機能は一通り揃っているので Chrome の拡張機能を多様していても、比較的移行しやすい環境だと感じます。
また Safari に Notion Web Clipper が対応したことで、ブラウザを選ぶことなく使用できるようになり、より Notion を使って Web Clip する機会が増えそうです。

Notion API の PublicBeta が公開されるなど、ここ最近で Notion はかなり注目を集めているので、今後もさらなるアップデートに期待したいところですね。

最後まで読んでいただきありがとうございました。