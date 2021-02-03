---
title: "Windows上でChromeOSを動かす [CloudReady]"
emoji: "☁"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["windows"]
published: false
---

# はじめに

ChromeBook には Google ChromeOS という OS が搭載されています。
これはオープンソースである ChromiumOS をベースに Google が設計した OS で、現在 Chromebook にて採用されていることで有名です。

UI がとてもシンプルで Google のサービスを利用するのにとても便利、困らなそうという印相を受けます。
Linux の機能がベータ版として組み込まれており。ターミナルを使った開発もできるようです。
ChromeBook の普及により今後、目にする機会や触る機会が多くなると思われます。

# Chrome OS を試してみたい

ChromeOS は ChromeBook にはプリインストールされている OS で試すには実機が必要と思われがちですが、neverware 社が提供してきる CloudReady というパッケージを使用することで簡単に個人利可能な ChromeOS をインストールできます。
CloudReady はオープンソースである Chrominuum OS をカスタマイズしたもので、ChromeOS と遜色なく使うことができます。

CloudReady の公式ページはこちらになります。

https://www.neverware.com/freedownload

この CloudReady を使用することで古い PC や Mac を簡単に ChromeBook 化できるということで注目を集めています。

つい先日、ChromeOS が CloudReady を買収し統合される方針のツイートがありました。

https://twitter.com/neverware/status/1338606293737730050?s=19

今後は CloudReady が正式に ChromeOS に生まれ変わる日も近いみたいですね。

今回は Windows にて仮想マシンを用意し、CloudReady を動かすところまでを解説します。

:::message
CloudReady は仮想環境で本番運用することを推奨していません。今回紹介する方法は検証用、動作チェックを想定して公開している仮想マシンイメージとなるため予めご了承ください。
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
途中 Linxu の拡張機能が促された場合合わせてインストールを行ってください。

![](https://storage.googleapis.com/zenn-user-upload/0n5mnvu4ocg77bisxeiyithku341)
_ようこそ！_

デフォルトでは言語が英語、キーボード配列が US 配列になっているため私は以下のように変更しました。

![](https://storage.googleapis.com/zenn-user-upload/f9zkp9v1usoa0jrj2q9ct9gzcz31) _使用しているキーボードが JIS 配列のため日本語を選択_

以上で CloudReady の導入が完了しました。

VMware は `Ctrl + Alt` でホスト OS(Windows) とゲスト OS(CloudReady) を行き来できるのが便利です。

初期設定で Google のアカウントにログインするとデスクトップが表示されます。
![](https://storage.googleapis.com/zenn-user-upload/fe7asv5fcidkgpue7z3f6mb7h3rs)
ブラウザとして Chromium(Chrome のオープンソース版)がインストールされています。

初期設定でログインした Google アカウントと紐付いているため自動的に Chrome の環境と同期が行われすぐに使い始めることができます。

# おわりに

仮想マシンで手軽に ChromeOS を動作させられるのが便利だと感じました。
今後 ChromeOS が CloudReady に買収されるなど ChromeOS が今後どんどん使いやすくなっていくと思われます。

今回紹介した方法はあくまで検証用や動作確認用ですが、手元に WindowsPC が一台あれば環境を大きく変えることなく試すことができるので ChromeOS に触ってみたい方には最適な手段だと感じました。

CloudReady は無料で使うことができ、仮想マシンではなく PC にメインの OS としてインストールもできるため、この機会に ChromeOS デビューをしてみてもいいと

最後まで読んでいただきありがとうございました。
