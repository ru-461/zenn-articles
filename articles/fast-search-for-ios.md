---
title: "ロック画面からでもSafariで高速検索したい【xSearch】"
emoji: "🔎"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["ios", "ipados", "safari", "ios15", "ipados15"]
published: true
---

## はじめに

日本時間2021年9月21日にAppleからiPhone、iPadを対象にiOS15・iPadOS15がリリースされました。全体的にマイナーアップデートと受け取られがちな今回のアップデートですが、SafariやSpotlight検索の機能強化が大幅に行われ使いやすくなっています。その中でも特に注目すべき機能強化が**Safariでの拡張機能サポート**と**ロック画面からのSpotlight検索へアクセス**だと私は考えています。あくまで個人的な意見ですが、今回のアップデートでかゆいところに手が届くのをすごく実感しました。

今回はSafariの拡張機能とSpotlight検索を組み合わせて高速で**カスタム検索**できる環境を構築してみました。

## カスタム検索とは

ブラウザの機能としてアドレスバーがURLを入力してのページと、検索エンジンを利用したキーワード検索、両方の役割を持つことが多くあります。Google ChromeやFirefox、Safariなどどのブラウザでもデフォルトでアドレスバーから検索が行えます。この際に利用する検索エンジンはブラウザによってデフォルトがGoogleだったり、Bingだったりと異なります。この検索エンジンはユーザーがカスタマイズできるようになっていることが多く、アドレスバーから特定のキーワードと検索文字列を利用して検索ができるようになっています。

検索エンジンというとGoogleやBingしか設定できないのかと思われがちですが、YouTubeやAmazonなど検索機能があるサイトであればカスタム検索エンジンとして追加可能です。

カスタムすることでアドレスバーから以下のような検索をすぐに行うことができるようになります。

![ChromeでのYouTube検索例の画像](/images/fast-search-for-ios/image01.png)
*アドレスバーからYouTubeを直接検索する*

Chromeだとその他の検索エンジンから以下の設定で特定のキーワード「`yt` + 任意の検索文字列」でアドレスバーから簡単にYouTubeの動画を検索できます。

![Chromeでの検索エンジン設定例の画像](/images/fast-search-for-ios/image02.png)
*PC版Chromeでのカスタム検索エンジン追加例*

このようにYouTubeの動画をアドレスバーから直接検索できるようになるため、時短に繋がります。

このような検索エンジンを自分好みにカスタマイズして使うことをカスタム検索ということが多いです。ざっくりとしてですが、ブラウザの検索エンジンをカスタムできることを利用して独自の検索条件をクエリとして追加することが多いです。

## Safariの拡張機能対応で手軽にカスタム検索が可能に

PC版のブラウザだと検索エンジンを柔軟に設定できたためかなり汎用性があるのですが、モバイル版のSafariやChromeだと同等の機能がなかったりと少し不便に感じることがありました。しかし今回のiOS15、iPadOS15にてmac向けのSafari拡張機能の一部が使用できるようになり、モバイル版のSafariでできることが大幅に増えました。

またSafariの検索はiPhone、iPadのホーム画面、ロック画面を下にスワイプすることで呼び出せるSpotlight検索から行えます。これはつまり、極端に言うなら**ロック画面からでも特定のサイト内でのダイレクトサーチが可能**になることを意味し、Spotlight検索とSafariを組み合わせた爆速検索が実現することになります。

下の例ではロック画面からSpotlight検索を使ってZennの[reactの検索結果](https://zenn.dev/search?q=react)を表示しています。登録した検索エンジンショートカット（zn）を検索文字列の前に付与することで
明示的に検索エンジンを指定するイメージです。SpotlightでSafariを呼び出していますが、これはSafariのアドレスバーへ直接入力しても機能します。

![Spotlight検索からZennにてreactを検索する様子の画像](/images/fast-search-for-ios/movie01.gif)
*Spotlight検索を使用してZennでキーワード「react」を検索する様子*

特定の検索サイト内に絞って検索できると情報へたどり着く時間がぐんと短縮できるため情報にたどり着く際の時短テクニックとして是非取り入れてほしいです。

## Safariでカスタム検索を可能にする「xSearch」とは

今回、カスタム検索をiPhone、iPadから行えるようにするため「xSearch for Safari」という拡張機能を導入します。
https://apps.apple.com/jp/app/xsearch-for-safari/id1579902068

日本円で**約400円**の有料拡張機能となるのですが、できることが一気に増えるため買う価値はあると感じます。探せば無料でカスタム検索ができる機能は他にもあります。この拡張機能はアップデートにてiCloudを使用した端末間の検索エンジン同期機能が先日実装されました。複数の端末を使用する上で同期機能が使えることは大きな武器になります。Apple製のデバイスを複数同時利用する、Safariをメインブラウザにするならより恩恵を受けられるように感じます。

## xSearchのインストール

Safariの拡張機能はAppStoreから導入ができます。拡張機能と呼ばれますが、通常のアプリと同じようにAppライブラリに格納されるため直感的にわかりやすくなっています。

AppStoreからインストールしたあとは通常のアプリと同様にAppライブラリの中に格納されます。

## 初期設定

xSearchは検索エンジンの追加をアプリ側で行います。あらかじめSafariで拡張機能の有効化を行っておきます。
設定から、Safari → 機能拡張と進みxSearchの機能を「ON」にします。

![拡張機能オプションの画像](/images/fast-search-for-ios/image03.png)

この状態でSafariを起動してページをリロードすることで拡張機能が有効になります。ここまで設定したらインストールした拡張機能のアプリを開いて検索エンジンの設定を実際に行っていきます。

## カスタム検索エンジンの導入

xSearchの初回起動時は以下の画面になります。

デフォルトですでにGoogleやBingなどのいくつか検索エンジンのサンプル設定がされています。

![検索エンジンの画像](/images/fast-search-for-ios/image04.png)
*デフォルトでいくつか設定されている*

この画面から右上の検索エンジン追加ボタンをタップして新規に検索エンジンを追加します。やり方はとても簡単で、右上のクリップアイコンから検索エンジン名（Engine Name）、呼び出しに使うショートカットキーワード（Shortcuts）、検索に使うURL（URL）を入れるだけです。

![エンジンタブの画像](/images/fast-search-for-ios/image06.png)
*ギャラリーからも追加可能*

PCのブラウザにて検索エンジンをカスタマイズしていたことがあれば、設定の項目、設定値をそのまま流用できるため直感的に理解しやすいと思われます。

ギャラリーには既に検索エンジンがいくつかテンプレートとして公開してあるため、こちらから直接追加できます。しかし日本語対応しているサイトが現時点ではほとんどないため、ドメインなどを、適宜変更する必要があります。ここのギャラリーにある設定例はアップデートによりどんどん増えているので気になったのがあればどんどん追加してみるのもいいです。直近だと先日までなかったDeepL翻訳が追加され翻訳の選択肢が増えました。追加する際は右にスワイプするだけで有効化してすぐ使えるようになります。

![ギャラリータブの画像](/images/fast-search-for-ios/image06.png)

実際にカスタム検索エンジンを追加してみます。例として[Zenn](https://zenn.dev)を追加してみます。

My Engineタブの右上から左の追加マークをタップし以下のように入力します。ドメイン、キーワード、検索エンジンのURLを入力します。

検索のトリガーとなるキーワード（ショートカット）は任意ですが、サイト名にちなんだ短いものをおすすめします。覚えやすく管理しやすいものが望ましいです。わたしはZennを略した`zn`をキーワードにしています。xSearchではショートカットに複数のキーワード指定が可能となっておりカンマ区切りで（zn, zenn）としても有効になります。

実際に使用する際はアドレスバーに「ショートカット + 検索文字列」と入力して利用します。

![検索エンジン追加画面の画像](/images/fast-search-for-ios/image07.png)

最後にURLのところに検索ページのURLを入力します。
検索ページのURLを調べるのは簡単で実際に検索キーワードで検索した結果ページのURLのキーワード部分（GETパラメータ）を`％s`で置き換えます。これで検索エンジンの追加が完了します。

URLによく使われるキーワードが候補として用意されており、タップで簡単に入力できるのが便利です。

## 参考：有名サイトの検索エンジン設定例

私が設定しているカスタムエンジンの設定例をご紹介します。私はかなりの数を登録しており、すべて載せるとかなりの量になるので、よく使うものからいくつか絞ってみました。

| 検索エンジン（Engine Name） | キーワード（Shortcuts） | URL |
| ---- | ---- | ---- |
| Zenn | zn | https://zenn.dev/search?q=%s |
| Qiita | qi | https://qiita.com/search?q=%s |
| note | no | https://note.com/search?q=%s |
| Amazon.co.jp | ama | https://www.amazon.co.jp/s?k=%s |
| YouTube | yt | https://youtube.com/results?search_query=%s |
| GitHub | gh | https://github.com/search?q=%s |
| MDN | mdn | https://developer.mozilla.org/search?q=%s |
| npm | npm | https://www.npmjs.com/search?q=%s |
| DeepL翻訳（→ 日本語） | dlj | https://www.deepl.com/ja/translator#../ja/%s |
| DeepL翻訳（→ 英語） | dle | https://www.deepl.com/ja/translator#../en/%s |
| Google翻訳（→ 日本語） | gtj | https://translate.google.com/?sl=auto&tl=ja&text=%s&op=translate |
| Google翻訳（→ 英語） | gte | https://translate.google.com/?sl=auto&tl=en&text=%s&op=translate |

上にサンプルとして上げていないものでも、検索ページのクエリを`%s`に置き換えることでほとんどのサイトに対応可能です。何を追加するべきか迷っている方の参考になれば幸いです。

また上の検索エンジンはPC版のChromeなどでも同じものが使えるため、自分なりに作成したものをドキュメントとしてまとめてどの環境でも設定できるようにするのがおすすめです。

## URLSCHEMEでの実行

URLSCHEME（URLスキーム）とは特定のURLを使用することでiOS、iPadのネイティブアプリを直接指定して開くことができるものになります。

アップデートによりどんどん追加されており、ギャラリーにもTwitter検索やYouTube検索のURLスキームがいくつか存在します。
上の検索エンジンをカスタマイズでは、ブラウザ版のサイトで検索されるのに対してこちらはアプリでの直接検索を可能にするというイメージです。

Safariで見ているサイトに対応するアプリがインストールされている場合はそこから起動できますが、直接アプリを起動して検索できるというのは場面によってはかなり便利です。URLスキームが使用できることでSpotlight検索からアプリの起動時のアクションを自在にカスタムできるためより便利に活用できるようになると感じます。便利な活用方法については模索中のため今後活用していく上で試行錯誤していきたいところです。

## おわりに

今回は、Safariの拡張機能対応で実現したカスタム検索をご紹介しました。今回紹介したxSearchは有料の拡張機能となりますが、iCloud同期への対応や頻繁なアップデートが行われているため今後さらに期待できそうです。

ギャラリーでもテンプレートとなる設定例が公開されており、直感的に追加して使用できるところがとても使いやすいと感じました。独自に検索エンジンを追加できるためカスタム検索エンジンをPCなどのブラウザと揃えられるのが日々使う中でとても便利に感じており重宝しています。

この拡張機能の登場はiOSでショートカット機能が実装された時を思わせるような衝撃を受けました。アイデア次第で活用の幅を広げてくれる可能性がある機能だと感じているため今後も更に便利な使い方を模索していきたいと感じます。

何事もそうですが便利な機能、生産性向上につながる機能はどんどん活用し情報をシェアしあっていきたいですね。

最後まで読んでいただきありがとうございました。
