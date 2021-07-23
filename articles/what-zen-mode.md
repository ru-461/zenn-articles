---
title: "VSCodeのZen Modeで集中力をハックする"
emoji: "🧘"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["vscode"]
published: true
---

# はじめに

Microsoft社が開発している[VSCode](https://azure.microsoft.com/ja-jp/products/visual-studio-code/) は拡張機能が豊富で柔軟なコーティング環境を作れるエディタでとても人気があります。また最近は同社が開発しているTypeScriptがフロントエンド開発のトレンドになってることもあり、VSCodeをメインのエディタとして使用することも多くなってきました。私自身、VSCodeにて記事を執筆していることもありいつもお世話になってます。

そこで今回は、コーティング作業へ集中することに特化した機能、**禅モード（Zen mode）** をご紹介します。

余談ではありますが、この記事を投稿しているこちらのサイト[Zenn](https://zenn.dev/) とは何も関係ありません。「Zen」です。

このサイト「Zenn」の名前の由来については過去に[catnoseさん](https://zenn.dev/catnose99)がdevsumi 2021にて言及されています。^[[Zennという名前にした理由](https://youtu.be/DTpGfpLybr0?t=1180)]

> 善い行いをすればいずれ返ってくる...  →  善  →  Zenn

この表現がサービスをうまく表現しているなと感じるとともに私がZennを好きな理由でもあります。

# Zen Modeの有効化

拡張機能の導入などは必要なく、VSCodeにデフォルトで用意されている機能なので、ショートカットで簡単に有効化できます。デフォルトでショートカットキーが用意されており、Windowsであれば`Ctrl`＋`K・Z` 、macOSであれば、`⌘`＋`K・Z`でZen Modeが発動します。

また`Ctrl`＋`Shift`＋`P` / `⌘`＋`Shift`＋`P`で開くコマンドパレットのウィンドウに、zenと入力すると有効化、無効化のメニューが表示され、こちらからも有効化できます。

![コマンドパレットから有効化する様子](https://storage.googleapis.com/zenn-user-upload/2f83d0d692a0b2cc190a388b.png)
*Zen Modeの切り替えを選択して有効化*

有効化すると一瞬でエディタ以外のインタフェースが消えました。これがいわゆる「Zen Mode」です。

![Zen Modeが有効化された様子](https://storage.googleapis.com/zenn-user-upload/1c3ca8a32d177846bc28dde3.png)*Zen ModeでUI表示が最低限になった*

海外のニュアンスなので少し解釈に迷いますが禅の精神（悟りをひらいた様子？）みたいなイメージでしょうか。とにかく集中だけに特化したモードといえますね。

この状態だとエディタにフォーカスが当たり続けるためコーティングだけに集中できるようになります。またサイドバーや設定メニューも隠れてしまいますが、VSCodeのショートカットキーを覚えていれば問題なく使用できるためそこまで大きな問題にはなりません。

意図的にではありますが、ショートカットキーを使って操作を強いられるようにもなるので自然とショートカットキーを覚えられるというのもいい点ですね。あえてこの環境でショートカットキーを勉強するのもいいなと感じました。

解除するときはEscキーを押すことでZen Modeが終了し通常モードに戻ります。

実際この記事もVSCodeのZenn Modeを使用して執筆しています。ターミナルもVSCode内からアクセスできるため、VSCodeの画面だけで作業が完結し、快適に執筆を進められるのがよかったです。

# 設定でさらにカスタマイズ

またZen modeは設定でさらにカスタマイズできます。初めてZen Modeを使うとき設定は以下のようになっています。

![設定がデフォルトの様子](https://storage.googleapis.com/zenn-user-upload/c112e2ba900d84b1c1ff9239.png)*設定変更前*

設定項目は日本語表記に対応しているため直感的に触ることができます。初期状態だとメインエディタが中央寄せ、ソースコードのないの行番号が非表示になるため私はしばらく使った上で以下のように設定しています。

![設定後の様子](https://storage.googleapis.com/zenn-user-upload/714c9d4b7e88315b0acbdf49.png)*設定変更後*

- Center Layout（レイアウトの中央寄せ）
- Hide Line Number（行番号の非表示）
- Hide Status Bar（下ステータスバーの非表示）

上の3項目それぞれデフォルトから変更しました。これにより最小限のUI表示を保ちながら作業に没入できるのでおすすめです。

![Zen Modeの設定変更後の様子](https://storage.googleapis.com/zenn-user-upload/8f4e4426dd7cf5473cf7701b.png)
*作業スペースが広くなるのでターミナルとエディタを並べても余裕*

他にもサイドバー、上部のUIの表示や、非表示を設定できます。
この状態でZen Modeの有効・無効を何回か切り替えてみるとコーディング環境が通常表示よりスリムになったのがよくわかります。

# おわりに

今回は、VSCodeでZen Modeを使用したコーディングだけに集中できる環境についてご紹介しました。

個人的にこのZen（禅）で集中力を高めるという発想がかなり気に入っています。小技的な機能にはなりますが、ショートカットキー1つでいつでも集中へ特化したレイアウトに変更できるのはとても便利です。またVSCodeは有志が開発している拡張機能が豊富で、VSCodeだけで完結できることが多くオールインワンエディタとしてのポテンシャルを秘めていると感じました。今回紹介したZen Mode以外にも便利な機能が多く、VSCodeを通して得られる**オールインワンな開発体験**にはすごく満足しています。

Zen ModeはJetbrains社の提供するPhpStorm^[[IDE 表示モード | PhpStorm](https://pleiades.io/help/phpstorm/ide-viewing-modes.html)] やPyCharm^[[IDE 表示モード | PyCharm](https://pleiades.io/help/pycharm/ide-viewing-modes.html)]などのIDEにも実装されているようです。他にも搭載しているエディタ、IDEがないか探してみるのも面白いですね。

最後まで読んでいただきありがとうございました。