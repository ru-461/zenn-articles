---
title: "Windows上でChrome OSを動かす [CloudReady]"
emoji: "⛅"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["windows", "chromeos", "cloudready", "vmware", "ツール"]
published: true
---

# はじめに

ChromeBook には Google ChromeOS という OS が搭載されています。
これはオープンソースである ChromiumOS をベースに設計されている OS で、現在 Chromebook にて採用されていることで有名です。

UI がとてもシンプルで洗練されており、Google のサービスを利用するのに便利、困らなそうという印象を受けました。
Linux の機能がベータ版として組み込まれており。ターミナルを使った開発もできるようです。
ChromeBook の普及により今後、目にする機会や触る機会が多くなると思われます。

# Chrome OS を試してみたい

ChromeOS は ChromeBook にはプリインストールされている OS で試すには実機が必要と思われがちですが、Neverware 社が提供してきる CloudReady というパッケージを使用することで個人利可能な ChromeOS ライクな環境を手に入れることができます。
CloudReady はオープンソースである Chrominuum OS をカスタマイズしたもので、ChromeOS と遜色なく使うことができます。

Cloudready は本家 ChromeOS と比較したときに Google Play ストアがサポートされていないため Android アプリをインストールして使うことができないという大きな違いがあります。
その他はそのまま ChromeOS と同じように使うことができます。

CloudReady の公式ページはこちらになります。

https://www.neverware.com/freedownload

この CloudReady を使用することで古い PC や Mac を簡単に ChromeBook 化できるということで注目を集めています。

つい先日、ChromeOS が CloudReady を買収し統合される方針のツイートがありました。

https://twitter.com/neverware/status/1338606293737730050?s=19

CloudReady が正式に ChromeOS として生まれ変わる日も近いみたいですね。

今回は Windows にて仮想マシンを用意し、CloudReady HomeEdition を動かすところまでを解説します。

:::message
後述しますが、本来 CloudReady は仮想環境で本番運用することを推奨していません。今回紹介する方法は検証用、動作チェックを想定して公開している仮想マシンイメージとなるため予めご了承ください。
:::

# CloudReady のインストール

## 仮想マシンの用意

まず ChromeOS(CloudReady)を Windows 上で動作させるために仮想マシンを用意します。
仮想マシンを利用することで Windows の環境を汚すことなく、OS を動かすことが可能となります。

CloudReady を仮想マシンで利用する場合は `VMware Workstation Player` での動作のみサポートされているため VMware Workstation Player を使用します。`VirtualBox` では上手く動作しない模様です。

VMware Workstation Player は以下のページからダウンロード可能です。

https://www.vmware.com/jp/products/workstation-player/workstation-player-evaluation.html

![](https://storage.googleapis.com/zenn-user-upload/w0r475gfqn6tcd0zvte1g9qr21a8)

VMware Workstation Player for Windows をインストールします。
VMware は商業利用ではなく個人利用であれば無償で利用できます。

![](https://storage.googleapis.com/zenn-user-upload/lik7ygtirmi2gvhvvljiwt079r48)

この画面が表示されれば VMware のインストール完了です。

## CloudReady の仮想マシンイメージのダウンロード

CloudReady の公式ページにて OS の VMware イメージが配布されているためダウンロードします。

https://cloudreadykb.neverware.com/s/article/Download-CloudReady-Image-For-VMware

こちらのページから VMware のイメージダウンロードできます。

![](https://storage.googleapis.com/zenn-user-upload/a01gdj2rxsbzroq83uonsihep83f)

ダウンロードリンクをクリックするとすぐにイメージのダウンロードが始まります。

## 仮想マシンイメージを VMware にインポート

先程、インストールした VMware に CloudReady のイメージを導入していきます。

VMware のトップ画面から `仮想マシンを開く` をクリック。
![](https://storage.googleapis.com/zenn-user-upload/2a737uds16ia5ssuglosc0dkzsrf)

表示されるダイアログでダウンロードした仮想マシンイメージを選択します。

![](https://storage.googleapis.com/zenn-user-upload/qenzprdht9uog3o4j5s3q8x9teiq)
_参照からパスを指定_

これでイメージの追加は完了です。

![](https://storage.googleapis.com/zenn-user-upload/yn5drm39iwry6xu3993ob1rxgtpc)
仮想マシンの設定を開くことで使うメモリの量やネットワークなどを詳しく設定できます。
ホストマシンの環境に応じて設定を変更することをおすすめします。

デフォルトでも問題なかったため、私はデフォルトで進めました。

## CloudReady の起動

`仮想マシンの再生`ボタンから起動できます。

![](https://storage.googleapis.com/zenn-user-upload/0n5mnvu4ocg77bisxeiyithku341)
_ようこそ！_

デフォルトでは言語が英語、キーボード配列が US 配列になっているため私は以下のように変更しました。

![](https://storage.googleapis.com/zenn-user-upload/f9zkp9v1usoa0jrj2q9ct9gzcz31) _使用しているキーボードが JIS 配列のため日本語を選択_

VMware は `Ctrl + Alt` でホスト OS(Windows) とゲスト OS(CloudReady) を行き来できるのが便利です。

初期設定で Google のアカウントにログインするとデスクトップが表示されます。
![](https://storage.googleapis.com/zenn-user-upload/fe7asv5fcidkgpue7z3f6mb7h3rs)
ブラウザとして Chromium(Chrome のオープンソース版)がインストールされています。

初期設定でログインした Google アカウントと紐付いているため自動的に Chrome の環境と同期が行われすぐに使い始めることができます。

以上で CloudReady の導入が完了です。

# 詰まったところと解決方法

## インターネットに繋がらない

私は PC を有線接続(イーサネット)で接続していたのですが、CloudReady の初期設定で上手く接続ができませんでした。
ChromeOS の初期設定をすすめる上でインターネット接続ができないと先に進めずつまずいたのですが、`仮想マシンの設定の編集` から `ネットワークアダプタ`を選択し。
![](https://storage.googleapis.com/zenn-user-upload/suxvg286i3rgqsm6q902d1sjhlh1)
`デフォルトのブリッジからNATに変更`することですんなりと繋がるようになりました。
繋がらない場合はお使いの環境に合わせてデフォルトから設定を変えてみてください。

## ロック解除に毎回パスワードを打つのが大変

ChromeOS ではログイン時に Google のパスワードを利用してロックを解除するのですが、私は Google のログインのパスワードに長い文字列を設定していたためロックのたびに入力するのが、がかなりストレスでした。`設定 → ユーザー → 画面ロック`で画面ロックの解除の方法をパスワードから 6 ケタの PIN に変更できます。

![](https://storage.googleapis.com/zenn-user-upload/9xcdmioqwe4x7ejftl9p21meed9c)

PIN にしてからストレスなくロックの解除ができるようになりました。
:::message
ロック解除に PIN を設定しても初回起動時のログインではパスワード入力が必須になります
:::

# Linux が起動できない問題

CloudReady(ChromeOS) にはターミナル機能と Linux の機能が搭載されています。
ターミナルは標準で搭載されており、`Ctrl + Shift + t`で呼び出すことができます。
![](https://storage.googleapis.com/zenn-user-upload/5c3ikwz48cj8vn7uvew8qh6hm27w)
それと合わせて使用できる Linux 機能が存在し、設定からインストールできるはずなのですが...。

![](https://storage.googleapis.com/zenn-user-upload/5s0a5bfhmxpdkzsobyrenu0ww7w6)

しかし仮想マシンで実行している CloudReady では設定画面に項目が存在するものの起動ができませんでした。
![](https://storage.googleapis.com/zenn-user-upload/5c3ikwz48cj8vn7uvew8qh6hm27w)
所持している別の PC にメイン OS としてインストールし実行したときは Linux とターミナルを普通に起動できたため仮想マシンでのサポートがされていないのではないかと感じました。

> Neverware never recommends running CloudReady as a VM for production use cases as the security and management benefits are reduced or eliminated when a host-OS is also involved.
> (公式ページから引用)

> Neverware は、ホスト OS も関与している場合、セキュリティと管理のメリットが減少または排除されるため、本番ユースケースの VM として CloudReady を実行することを推奨しません。

公式の VMware イメージ配布ページでもこのように説明がされている通り、やはり仮想マシンでは Linux の実行は現実的で無いような気がします。
ChromeOS で Linux を使って開発環境を構築している情報も多く見受けられますが、CloudReady 開発マシンとして運用する場合は仮想マシンではなく、メインの OS として正規にインストールするしかないみたいです。

上記を踏まえても、仮想マシンでの運用は動作確認と検証用に留めるのが理想的だと感じました。
CloudReady を触っていく中でこの部分についても引き続き検証していきたいです。

# おわりに

仮想マシンで手軽に ChromeOS の環境を動作させられるのが便利だと感じました。
今後 ChromeOS が CloudReady に買収されるなど Chrome OS が今後どんどん使いやすくなっていくと思われます。

今回紹介した方法はあくまで検証用や動作確認用ですが、手元に WindowsPC が 1 台あれば環境を大きく変えることなく試すことができるので ChromeOS に触ってみたい方には最適な手段だと感じました。今回紹介しているのは仮想マシンでの利用ですが、CloudReady 側としては PC にメインの OS としてインストールするのを推奨しているため今度は、メイン OS として使ってみるのも面白そうです。

昨今はブラウザベースで利用できるサービスも多くあります。Notion を使ったドキュメント編集などのブラウザベースの作業をするのに困ることはなさそうです。

この機会に CloudReady を使って ChromeOS デビューをしてみるのはいかがでしょうか。

最後まで読んでいただきありがとうございました。

# 参考ドキュメント

[Chromium OS 派生　 CloudReady - なろぐ 2](https://narolll.hateblo.jp/entry/20201027/1603784651)
[グーグル、「Chromium」ベースの OS「CloudReady」の Neverware 買収 - ZDNet Japan](https://japan.zdnet.com/article/35164052/)
