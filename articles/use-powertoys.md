---
title: "Windowsのポテンシャルをさらに引き出すツール群「PowerToys」"
emoji: "🧮"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["windows", "powertoys", "microsoft"]
published: true
---

# PowerToysとは？

[PowerToys](https://docs.microsoft.com/ja-jp/windows/powertoys/)とはMicrodoftが開発しているWindowsの拡張ユーティリティ郡になります。もともとWindowsに搭載される予定だった機能が、別途まとめて配信されているものとなり、現在オープンソース化されて開発されています。

https://github.com/microsoft/PowerToys

現在Windows10の機能はアップデートでどんどん改良され、リリースされた当時に比べると全体的にかなり使いやすくなってきました。しかしまだ完全ではなく微妙に使いにくいと感じる部分も多くあります。Windowsには多くのフリーソフトが存在するため、各自で不満点を補うことが簡単にできるのが強みです。そこでMicrosoftもPowerToysというユーティリティを開発し提供しており、開発を進めています。またMicrosoftが開発している公式の拡張ユーティリティ群になるのでシステムから簡単に呼び出せたりといったWindowsとの親和性の高さと安定感があります。

提供されている機能はPowerToysのアップデートにより、どんどん追加され、Windowsに今後標準搭載される可能性もあります。

:::message
この記事内ではPowerToys **v0.33.1時点**で含まれる機能について紹介しています。PowerToysは現在開発途中のアプリケーションのため、今後アップデートにより提供される機能や、仕様が大きく変わることもあります。
:::

# PowerToysで提供される機能一覧

| 機能名 | 説明 |
| ---- | ---- |
| Color Picker | カラーピッカーツール |
| FancyZones | ウィンドウの分割ツール |
| エクスプローラー | 標準のエクスプローラーに機能を追加 |
| Image Resizer | 画像のリサイズツール |
| KeyBoard Manager | キーボードの再マッパ |
| PowerRename | ファイル名一括編集ツール |
| PowerToys Run | 検索＆ランチャー |
| Shortcut Guide | Windowsキー のショートカットガイド |

PowerToysをインストールすることでこれらの機能が一気に使えるようになります。

# インストール

PoweToysはGitHub上のリリースページから最新バージョンのセットアップ実行ファイルをダウンロードしてインストールできます。

Assetsの中にある**PowerToysSetup-〇〇〇-x64.exe**をクリックするとダウンロードが始まります。〇にはインストールされるバージョンが入ります。

https://github.com/microsoft/PowerToys/releases/

インストールはプログラムのインストール場所と簡単なオプションを選択するだけで終わります。

インストール途中でユーザーアカウント制御の画面が現れたら許可をしてください。インストーラーの中で「Automatically start PowerToys at logon」を選択しておくとWindowsへログオンした時に自動的に起動するようになります。

また[Windows Package Manager](https://github.com/microsoft/winget-cli)を導入している場合は以下のコマンドをコマンドプロンプトやPowerShell上から実行することで簡単にインストールできます。

```powershell:PowerShell
> WinGet install powertoys
```

インストールし起動するとタスクトレイからPowerToysのアイコンが現れます。アイコンをクリックして以下のような設定画面が表示されたらインストールは成功しています。

![デフォルトの設定画面の画像](https://storage.googleapis.com/zenn-user-upload/x14yaeyvet2fouzewzpha39ubahn)
*インストール直後からすべて日本語化されている*

再起動をすることなくすぐにPowerToysを使い始めることができます
設定画面からはそれぞれの機能を呼び出すショートカットキーを始め、機能を細かくカスタマイズできます。

# 追加される機能

## ColorPicker

Windows内でいつでも呼び出せるカラーピッカーです。画面の中の要素にカーソルを合わせることで色のカラーコードを取得できます。
Chromeの拡張機能でよく目にするカラーピッカーですが、Windowsの標準の機能としてカラーピッカーが使えるようになるのは大きいです。

使用するには設定ウィンドウからColorPickerを呼び出すショートカットを設定する必要があります。デフォルトでは`Windows`＋`Shift`＋`C`で呼び出せるように設定されています。

試しにWindowsのデスクトップ壁紙のカラーコードを取得してみたところ。

![壁紙の色コードを取得する様子](https://storage.googleapis.com/zenn-user-upload/u9jc1jw6zo3q15ng23lewelm92pj)

カーソルのあたった箇所のカラーコードを取得できました。取得した色についての詳細の表示や、色の編集も可能です。

## FancyZones

ウィンドウをレイアウトを分けてきれいに配置するためのアシストツールになります。
マルチタスクのために画面を分割して使うときに威力を発揮します。

まずレイアウトエディターでレイアウトを設定します。レイアウトエディタは`Windows`＋`@`で呼び出せます。
設定できるレイアウトはいくつかレイアウトがテンプレートとして用意されています。テンプレート以外にも各自でレイアウトを新規に定義もできます。

![レイアウトエディタの画面](https://storage.googleapis.com/zenn-user-upload/q07csh1x16dhr6bhl10qbm272mzw)

ここで設定したレイアウトに沿ってウィンドウの配置を簡単に行えるようになります。デフォルトでは`Shift`を押したままウィンドウをドラッグすることで指ウィンドウを柔軟に配置することが可能になります。

## エクスプローラー

Windows標準のファイル管理ツールエクスプローラーに機能を追加できます。

[powertoys v0.33.1](https://github.com/microsoft/PowerToys/releases) 時点では以下の項目が追加可能です。

それぞれの機能は設定から個別にオン・オフできます。

![エクスプローラー設定の画面](https://storage.googleapis.com/zenn-user-upload/bc7u7j32q8fkxcekdl1550u36ms2)

上の例では、プレビュー可能ファイルにSVGファイル（.svg）とマークダウンファイル（.md）を追加しています。またアイコンプレビューとしてSVGファイルがサムネイルとして表示されるようになります。
現時点では標準でプレビューに表示されないマークダウンファイルが手軽にプレビューできるようになるので、Windows上でマークダウンファイルを扱う人におすすめです。

これらの機能は今後標準のエクスプローラーにも実装されそうですね。

## Image Resizer

画像リサイズ機能を右クリックメニューへ追加し、エクスプローラーから画像のリサイズを簡単に行えるようにします。

使い方は簡単で、右クリックで表示されるコンテキストメニューから画像のサイズを変更を選択するだけです。

![コンテキストメニューの様子](https://storage.googleapis.com/zenn-user-upload/qffz9esgpkbxvpqdu2q1ctiqbvl9)

すると以下のような選択した画像をリサイズするウィンドウが開かれるので、ここから自由に画像のサイズ変更を行えます。

![リサイズメニューの様子](https://storage.googleapis.com/zenn-user-upload/5w3aoy4rsuau8yq8lkjyc2no03s1 =520x)

予めよく使われる解像度がプリセットで用意されているので簡単に目的のサイズにリサイズできるのはとても便利です。またカスタムとして各々が指定した数値でのリサイズも可能です。

## Keyboard Manager

キーボードの再マッピング（再割当て）を行えるツールです。

設定からキーの再マップを選択することで、キーの再マップウィンドウが開きます。
使わないキーの無効化や入力されるキーの変更を簡単に行えるため、便利です。

## PowerRename

ファイルの名前を一括して置換する強力なツールです。

使い方は簡単でエクスプローラから編集したいファイルを複数選択し右クリックのコンテキストメニューから「Powerrename」を選択することで編集ウィンドウが開きます。

![Powerremoveウィンドウの画像](https://storage.googleapis.com/zenn-user-upload/xlo8thjodz666pnjva92rv8l2si5 =520x)

まとめてファイル名を変更でき、おなじみの正規表現も使用できるため、柔軟にファイル名を編集できます。

## PowerToys Run

機能を有効化することで、macOSなどでおなじみのSpotlight検索ライクな検索＆アプリランチャーが使用できるようになります。
MacにはSpotlight検索を便利した[Alfred](https://www.alfredapp.com/)という有名なツールがあります。PowertoysRunを使うことで完全とはいきませんが、近い操作感をWindowsでも実現させることが可能になります。

デフォルトでは`Alt`＋`Space`で呼び出すように設定されています。ショートカットキーを実行すると以下のようなウィンドウが画面上に現れます。

![表示されたウインドウの画像](https://storage.googleapis.com/zenn-user-upload/vi1e83i0ptyypuswfxwf2h18sgfn)

例としてインストール済みのアプリを検索してみます。検索したい文字列を途中まで入れるとその文字列を含むアプリを列挙してくれます。
![検索結果の画像](https://storage.googleapis.com/zenn-user-upload/1qxl8zxk8g1ilge0tgc2vwx9v4l4)
*Visual Studioと検索した結果*

検索結果をカーソルキーで移動してエンターまたは、クリックで実行できます。
プラグインが用意されており、設定画面から個別に有効・無効を切り替えることができます。

プラグインには、簡単な電卓代わりとして使用可能とする計算ツールやプログラムを検索対象に含めるプログラム、ウィンドウから直接コマンドをシェルで実行させるシェルなどがあります。
設定画面からは検索結果に表示する件数や呼び出すショートカットキーの設定など細かくカスタマイズできます。個々の項目は今後のアップデートでどんどん変わるため、各々好みに合わせて変更してみてください。

まだ発展途上の機能にはなりますが、この機能は今後、Windows標準のツールとしてぜひ取り入れて欲しいものです。
## Shortcut Guide

有効化することでWindowsキーを長押したときに使用できるショートカットがオーバーレイとして表示されるようになります。

![ショートカットガイドを表示させた様子](https://storage.googleapis.com/zenn-user-upload/cmuozzbjf7w62p6r745mti8aqs8j)

例えば、`Windows`＋`矢印`でウィンドウのスナップ、`Windows`＋`A`でアクションセンターを開くなど。
Windowsキーが絡むショートカットキーは組合わせが覚えにくいものもあるので、すぐに使用できるキーの組み合わせを表示できるのは便利です。

# おわりに

今回は、Windows公式のユーティリティ群である「PowerToys」を紹介しました。
今回紹介したツールはPowerToysをインストールするだけで再起動することなく、すぐに使用可能になるのでとても便利です。PowerToysで追加された機能は今後Windowsのアップデートにより、正式に追加されることもあるため、どんどん便利になっていくのを見ているだけでもわくわくします。

PowerToysを使うことでいつものWindowsの作業が少し便利に楽しくなるので気になったら是非、使ってみてください。

最後まで読んでいただきありがとうございました。

# 参考

- [Microsoft公式ツール「Powertoys」を解説。なんでこの機能、Windowsに標準で入ってないの？という神機能が特盛！ – すまほん!!](https://smhn.info/202101-how-to-use-microsoft-windows-powertoys#i-3)