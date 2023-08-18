---
title: "SafariがNotion Web Clipperに対応したので使ってみる"
emoji: "🧭"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["notion", "safari", "拡張機能"]
published: true
---

# はじめに

Notionには公式の拡張機能として[Notion Web Clipper](https://www.notion.so/web-clipper)というものがあります。

![Notion Web Clipperのトップページ画像](/images/web-clipper-with-safari/image01.png)

Web Clipperという名の通り、ブラウザで今見ているWebページをNotionのページに変換し保存できるすぐれものです。ChromeやFirefoxでは[拡張機能](https://chrome.google.com/webstore/detail/notion-web-clipper/knheggckgoiihginacbkhaalnibhilkk)・[アドオン](https://addons.mozilla.org/ja/firefox/addon/notion-web-clipper)として前から提供されていましたが、Safariでは使用できない状態となっていました。モバイル版Safari（iPhone・iPad）からは共有機能を使ってNotionを開くことでWeb Clipperが使用できていましたが、PC版のSafariには対応していませんでした。

私は、MacのSafariをあまり使うことがなかったのですが、先日、Notion Web ClipperがSafariに対応したと聞き、実際にSafariへ導入してしばらく使ってみました。

https://twitter.com/NotionHQ/status/1387825941263486976

Notion Web ClipperはNotionを使うならぜひとも合わせて使いたいくらいに便利なものなのでご紹介します。

# Safariに拡張機能を導入する

あまり知られていないように感じますが、SafariもChromeやFirefoxと同様に拡張機能を導入することで、さらに機能を増やすことができます。Safariへ拡張機能を追加する際はMacのAppStoreから拡張機能を入手しインストールする形となります。

## 導入済みの拡張機能を管理する

Safariの設定画面を開き、拡張機能の項目を開くことで現在Safariに導入している拡張機能を確認できます。

![拡張機能の管理画面画像](/images/web-clipper-with-safari/image02.png)
*導入しているアプリによって拡張機能が入っている場合もあります*

ここに拡張機能を検索して追加することで管理が可能となります。チェックマークの付け外しで有効無効の切り替えを行ったり、アンインストールもここから行うことができます。

また右下には「拡張機能を追加...」というボタンがあり、ここからAppStoreのSafari拡張機能カテゴリページを開くことができます。
## Mac AppStoreで拡張機能を探す

Mac AppStoreのサイドメニューからカテゴリを選択すると一覧の中にSafari拡張機能があります。

![拡張機能カテゴリの画像](/images/web-clipper-with-safari/image03.png)

ここにSafariで使用可能な拡張機能がまとまっています。無料のものから有料のものまで数多くありますが、ランキング形式で見やすくまとめられているのが探しやすくて便利です。Safariの拡張機能は、通常のMacアプリと同じようにインストールが可能で、デフォルトでアプリケーションフォルダ（~/Applications）の中に格納されます。

# Notion Web ClipperをSafariにインストール

それでは、SafariにNotion Web Clipperをインストールしていきます。
Mac AppStore内の検索から「Notion」と検索することで上位にヒットします。

![Notion Web Clipperの画像](/images/web-clipper-with-safari/image04.png)
*インストール済みの場合は「開く」に変わる*

または以下のリンクから直接、ダウンロードページを開くことができます。

https://apps.apple.com/jp/app/notion-web-clipper/id1559269364?mt=12

拡張機能の詳細ページを開いたら通常のアプリと同じようにインストールをクリックして導入します。サインインが求められた場合は、パスワードまたは生体認証にてサインインすることでインストールが始まります。

インストールした後はSafariの設定から拡張機能を開くことで有効 / 無効を切り替えられるようになります。インストールしてもうまく反映されない場合は拡張機能が有効になっているかどうかを見直してみてください。

# Notion Web ClipperでWebページをクリップする

ここからはChromeやFirefoxと共通の操作になります。

拡張機能を導入するとSafariの上部メニューにNotionのアイコンが表示されるようになり、ここからNotion Web Clipperにアクセスできます。

初回起動時はNotionのログインページへ促されNotionのアカウントでログインする画面になります。ログイン後、Webページを開いた状態でNotion Web Clipperを起動すると以下のような表示になります。

![Webページをクリップする様子](/images/web-clipper-with-safari/image05.png)
*Notion Web Clipper のページをクリップする様子*

Notionにログインしてあるので、Workspaceと保存先のページを選ぶ項目が表示されています。

上の例だとMySpaceというWorkspace内にあるWebClipsというデータベースに保存するという意味になります。Webページを保存する用のデータベースページを予め作成しておくのがおすすめです。この状態でSave pageボタンをクリックすることで瞬時に指定したデータベースへWebページが保存されます。

保存したあとに表示されるOpen in NotionをクリックすることでクリップしたページをすぐにNotionで開いて確認できるのが便利です。

# おわりに

今回、Safariの拡張機能とNotion Web Clipperを導入する方法について書きました。初めてSafariに拡張機能を導入しましたが、SafariもChromeと同じように拡張機能を導入できるのが便利だと感じました。有名な拡張機能は一通り揃っているのでChromeの拡張機能を多様していても、比較的移行しやすい環境だと感じます。

またSafariにNotion Web Clipperが対応したことで、ブラウザを選ぶことなく使用できるようになり、よりNotionを使ってWeb Clipする機会が増えそうです。

今後もさらなるアップデートに期待したいところですね。

最後まで読んでいただきありがとうございました。
