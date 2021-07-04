---
title: "Windows上でChrome OSを動かす【CloudReady】"
emoji: "⛅"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["windows", "chromeos", "cloudready", "vmware", "ツール"]
published: true
---

# はじめに

ChromeBookにはGoogle ChromeOSというOSが搭載されています。
これはオープンソースであるChromiumOSをベースに設計されているOSで、現在Chromebookにて採用されていることで有名です。

UIがとてもシンプルで洗練されており、Googleのサービスを利用するのに便利、困らなそうという印象を受けました。
Linuxの機能がベータ版として組み込まれており。ターミナルを使った開発もできるようです。
ChromeBookの普及により今後、目にする機会や触る機会が多くなると思われます。

# Chrome OS を試してみたい

ChromeOSはChromeBookにはプリインストールされているOSで試すには実機が必要と思われがちですが、Neverware社が提供してきるCloudReadyというパッケージを使用することで個人利可能なChromeOSライクな環境を手に入れることができます。
CloudReadyはオープンソースであるChrominuum OSをカスタマイズしたもので、ChromeOSと遜色なく使うことができます。

Cloudreadyは本家ChromeOSと比較したときにGoogle PlayストアがサポートされていないためAndroidアプリをインストールして使うことができないという大きな違いがあります。
その他はそのままChromeOSと同じように使うことができます。

CloudReadyの公式ページはこちらになります。

https://www.neverware.com/freedownload

このCloudReadyを使用することで古いPCやMacを簡単にChromeBook化できるということで注目を集めています。

つい先日、ChromeOSがCloudReadyを買収し統合される方針のツイートがありました。

https://twitter.com/neverware/status/1338606293737730050?s=19

CloudReadyが正式にChromeOSとして生まれ変わる日も近いみたいですね。

今回はWindowsにて仮想マシンを用意し、CloudReady HomeEditionを動かすところまでを解説します。

:::message
後述しますが、本来CloudReadyは仮想環境で本番運用することを推奨していません。今回紹介する方法は検証用、動作チェックを想定して公開している仮想マシンイメージとなるため予めご了承ください。
:::

# CloudReady のインストール

## 仮想マシンの用意

まずChromeOS（CloudReady）をWindows上で動作させるために仮想マシンを用意します。
仮想マシンを利用することでWindowsの環境を汚すことなく、OSを動かすことが可能となります。

CloudReadyを仮想マシンで利用する場合は`VMware Workstation Player`での動作のみサポートされているためVMware Workstation Playerを使用します。`VirtualBox`では上手く動作しない模様です。

VMware Workstation Playerは以下のページからダウンロード可能です。

https://www.vmware.com/jp/products/workstation-player/workstation-player-evaluation.html

![VMware Workstation Playerをインストールする様子](https://storage.googleapis.com/zenn-user-upload/w0r475gfqn6tcd0zvte1g9qr21a8)

VMware Workstation Player for Windowsをインストールします。
VMwareは商業利用ではなく個人利用であれば無償で利用できます。

![VMware Workstation Player起動後の画像](https://storage.googleapis.com/zenn-user-upload/lik7ygtirmi2gvhvvljiwt079r48)

この画面が表示されればVMwareのインストール完了です。

## CloudReady の仮想マシンイメージのダウンロード

CloudReadyの公式ページにてOSのVMwareイメージが配布されているためダウンロードします。

https://cloudreadykb.neverware.com/s/article/Download-CloudReady-Image-For-VMware

こちらのページからVMwareのイメージダウンロードできます。

![イメージをダウンロードするリンクの画像](https://storage.googleapis.com/zenn-user-upload/a01gdj2rxsbzroq83uonsihep83f)

ダウンロードリンクをクリックするとすぐにイメージのダウンロードが始まります。

## 仮想マシンイメージを VMware にインポート

先程、インストールしたVMwareにCloudReadyのイメージを導入していきます。

VMwareのトップ画面から`仮想マシンを開く`をクリック。

![仮想マシンを開くボタンの画像](https://storage.googleapis.com/zenn-user-upload/2a737uds16ia5ssuglosc0dkzsrf)

表示されるダイアログでダウンロードした仮想マシンイメージを選択します。

![イメージを指定するパスを](https://storage.googleapis.com/zenn-user-upload/qenzprdht9uog3o4j5s3q8x9teiq)
*参照からパスを指定*

これでイメージの追加は完了です。

![仮想マシン設定画面の画像](https://storage.googleapis.com/zenn-user-upload/yn5drm39iwry6xu3993ob1rxgtpc)

仮想マシンの設定を開くことで使うメモリの量やネットワークなどを詳しく設定できます。
ホストマシンの環境に応じて設定を変更することをおすすめします。

デフォルトでも問題なかったため、私はデフォルトで進めました。

## CloudReady の起動

`仮想マシンの再生`ボタンから起動できます。

![CloudReadyようこそ画面の画像](https://storage.googleapis.com/zenn-user-upload/0n5mnvu4ocg77bisxeiyithku341)
*ようこそ！*

デフォルトでは言語が英語、キーボード配列がUS配列になっているため私は以下のように変更しました。

![言語とキーボード変更画面の画像](https://storage.googleapis.com/zenn-user-upload/f9zkp9v1usoa0jrj2q9ct9gzcz31) *使用しているキーボードが JIS 配列のため日本語を選択*

VMwareは`Ctrl + Alt`でホストOS（Windows）とゲストOS（CloudReady）を行き来できるのが便利です。

初期設定でGoogleのアカウントにログインするとCloudReadyのデスクトップが表示されます。

![CloudReadyのデスクトップ画像](https://storage.googleapis.com/zenn-user-upload/fe7asv5fcidkgpue7z3f6mb7h3rs)

ブラウザとしてChromium（Chromeのオープンソース版）がインストールされています。

初期設定でログインしたGoogleアカウントと紐付いているため自動的にChromeの環境と同期が行われすぐに使い始めることができます。

以上でCloudReadyの導入が完了です。

# 詰まったところと解決方法

## インターネットに繋がらない

私はPCを有線接続（イーサネット）で接続していたのですが、CloudReadyの初期設定で上手く接続ができませんでした。
ChromeOSの初期設定をすすめる上でインターネット接続ができないと先に進めずつまずいたのですが、`仮想マシンの設定の編集`から`ネットワークアダプタ`を選択し。

![ブリッジ接続からNATへ変更する様子](https://storage.googleapis.com/zenn-user-upload/suxvg286i3rgqsm6q902d1sjhlh1)

`デフォルトのブリッジからNATに変更`することですんなりと繋がるようになりました。
繋がらない場合はお使いの環境に合わせてデフォルトから適宜設定を変えてみてください。

## ロック解除に毎回パスワードを打つのが大変

ChromeOSではログイン時にGoogleのパスワードを利用してロックを解除するのですが、私はGoogleのログインのパスワードに長い文字列を設定していたためロックのたびに入力するのが、がかなりストレスでした。`設定 → ユーザー → 画面ロック`で画面ロックの解除の方法をパスワードから6ケタのPINに変更できます。

![PINを設定する様子](https://storage.googleapis.com/zenn-user-upload/9xcdmioqwe4x7ejftl9p21meed9c)

PINにしてからストレスなくロックの解除ができるようになりました。

:::message
ロック解除にPINを設定しても初回起動時のログインではパスワード入力が必須になります。
:::

# Linux が起動できない問題

CloudReady（ChromeOS）にはターミナル機能とLinuxの機能が搭載されています。
ターミナルは標準で搭載されており、`Ctrl + Shift + t`で呼び出すことができます。

![CloudReadyのターミナル画像](https://storage.googleapis.com/zenn-user-upload/5c3ikwz48cj8vn7uvew8qh6hm27w)

それと合わせて使用できるLinux機能が存在し、設定からインストールできるはずなのですが...。

![Linuxが起動できない様子](https://storage.googleapis.com/zenn-user-upload/5s0a5bfhmxpdkzsobyrenu0ww7w6)

しかし仮想マシンで実行しているCloudReadyでは設定画面に項目が存在するものの起動ができませんでした。

所持している別のPCにメインOSとしてインストールし実行したときはLinuxとターミナルを普通に起動できたため仮想マシンでのサポートがされていないのではないかと感じました。

> Neverware never recommends running CloudReady as a VM for production use cases as the security and management benefits are reduced or eliminated when a host-OS is also involved.
> （公式ページから引用）

> Neverware は、ホスト OS も関与している場合、セキュリティと管理のメリットが減少または排除されるため、本番ユースケースの VM として CloudReady を実行することを推奨しません。

公式のVMwareイメージ配布ページでもこのように説明がされている通り、やはり仮想マシンではLinuxの実行は現実的で無いような気がします。
ChromeOSでLinuxを使って開発環境を構築している情報も多く見受けられますが、CloudReady開発マシンとして運用する場合は仮想マシンではなく、メインのOSとして正規にインストールするしかないみたいです。

上記を踏まえても、仮想マシンでの運用は動作確認と検証用に留めるのが理想的だと感じました。
CloudReadyを触っていく中でこの部分についても引き続き検証していきたいです。

# おわりに

仮想マシンで手軽にChromeOSの環境を動作させられるのが便利だと感じました。
今後ChromeOSがCloudReadyに買収されるなどChrome OSが今後どんどん使いやすくなっていくと思われます。

今回紹介した方法はあくまで検証用や動作確認用ですが、手元にWindowsPCが1台あれば環境を大きく変えることなく試すことができるのでChromeOSに触ってみたい方には最適な手段だと感じました。今回紹介しているのは仮想マシンでの利用ですが、CloudReady側としてはPCにメインのOSとしてインストールするのを推奨しているため今度は、メインOSとして使ってみるのも面白そうです。

昨今はブラウザベースで利用できるサービスも多くあります。Notionを使ったドキュメント編集などのブラウザベースの作業をするのに困ることはなさそうです。

この機会にCloudReadyを使ってChromeOSデビューをしてみるのはいかがでしょうか。

最後まで読んでいただきありがとうございました。

# 参考ドキュメント

- [Chromium OS 派生 CloudReady - なろぐ 2](https://narolll.hateblo.jp/entry/20201027/1603784651)
- [グーグル、「Chromium」ベースの OS「CloudReady」の Neverware 買収 - ZDNet Japan](https://japan.zdnet.com/article/35164052/)