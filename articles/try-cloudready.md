---
title: "Windows上でChromeOSを動かす【CloudReady】"
emoji: "⛅"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["windows", "chromeos", "cloudready", "vmware", "ツール"]
published: true
---

## はじめに

ChromeBookにはChromeOSというOSが搭載されています。
これはオープンソースであるChromiumOSをベースに設計されているOSで、現在Chromebookにて採用されていることで有名です。

UIがとてもシンプルで洗練されており、Googleのサービスを利用するのに便利、困らなそうという印象を受けました。
Linuxの機能がベータ版として組み込まれており。ターミナルを使った開発もできるようです。
ChromeBookの普及により今後、目にする機会や触る機会が多くなると思われます。

## ChromeOSを試してみたい

この記事内ではNeverware社が提供しているCloudReadyというパッケージを使用してChromeOSライクな環境を構築していきます。
またCloudReadyはオープンソースであるChrominuumOSをカスタマイズしたものになるので、ChromeOSと遜色なく使うことができます。

Cloudreadyは本家ChromeOSと比較したときにGoogle PlayストアがサポートされていないためAndroidアプリをインストールして使うことができないという大きな違いがあります。
その他はそのままChromeOSと同じように使うことができます。

CloudReadyの公式ページはこちらになります。
https://www.neverware.com/freedownload

このCloudReadyを使用することでPCを簡単にChromeBook化できるということで注目を集めています。

つい先日、ChromeOSがCloudReadyを買収し統合される方針のツイートがありました。
https://x.com/neverware/status/1338606293737730050?s=19

CloudReadyが正式にChromeOSとして生まれ変わる日も近いみたいですね。

今回はWindowsにて仮想マシンを用意し、CloudReady HomeEditionを動かすところまでを解説します。

:::message
後述しますが、本来CloudReadyは仮想環境で本番運用することを推奨していません。今回紹介する方法は検証用、動作チェックを想定して公開している仮想マシンイメージとなるため予めご了承ください。
:::

## CloudReadyのインストール

### 仮想マシンの用意

まずChromeOS（CloudReady）をWindows上で動作させるために仮想マシンを用意します。
仮想マシンを利用することでWindowsの環境を汚すことなく、OSを動かすことが可能となります。

CloudReadyを仮想マシンで利用する場合はVMware Workstation Playerでの動作のみサポートされているためVMware Workstation Playerを使用します。VirtualBoxでは上手く動作しない模様です。

VMware Workstation Playerは以下のページからダウンロード可能です。
https://www.vmware.com/jp/products/workstation-player/workstation-player-evaluation.html

![VMware Workstation Playerをインストールする様子](/images/try-cloudready/image01.png)

VMware Workstation Player for Windowsをインストールします。
VMwareは商業利用ではなく個人利用であれば無償で利用できます。

![VMware Workstation Player起動後の画像](/images/try-cloudready/image02.png)

この画面が表示されればVMwareのインストール完了です。

### 仮想マシンイメージのダウンロード

CloudReadyの公式ページにてOSのVMwareイメージが配布されているためダウンロードします。

https://cloudreadykb.neverware.com/s/article/Download-CloudReady-Image-For-VMware

こちらのページからVMwareのイメージダウンロードできます。

![イメージをダウンロードするリンクの画像](/images/try-cloudready/image03.png)

ダウンロードリンクをクリックするとすぐにイメージのダウンロードが始まります。

### 仮想マシンイメージをVMwareにインポート

先程、インストールしたVMwareにCloudReadyのイメージを導入していきます。

VMwareのトップ画面から仮想マシンを開くをクリック。

![仮想マシンを開くボタンの画像](/images/try-cloudready/image04.png)

表示されるダイアログでダウンロードした仮想マシンイメージを選択します。

![イメージを指定するパスを](/images/try-cloudready/image05.png)
*参照からパスを指定*

これでイメージの追加は完了です。

![仮想マシン設定画面の画像](/images/try-cloudready/image06.png)

仮想マシンの設定を開くことで使うメモリの量やネットワークなどを詳しく設定できます。
ホストマシンの環境に応じて設定を変更することをおすすめします。

デフォルトでも問題なかったため、私はデフォルトで進めました。

### CloudReadyの起動

仮想マシンの再生ボタンから起動できます。

![CloudReadyようこそ画面の画像](/images/try-cloudready/image07.png)
*ようこそ!*

デフォルトでは言語が英語、キーボード配列がUS配列になっているため私は以下のように変更しました。

![言語とキーボード変更画面の画像](/images/try-cloudready/image08.png) *使用しているキーボードがJIS配列のため日本語を選択*

VMwareは`Ctrl`＋`Alt`でホストOS（Windows）とゲストOS（CloudReady）を行き来できるのが便利です。

初期設定でGoogleのアカウントにログインするとCloudReadyのデスクトップが表示されます。

![CloudReadyのデスクトップ画像](/images/try-cloudready/image09.png)

ブラウザとしてChromium（Chromeのオープンソース版）がインストールされています。

初期設定でログインしたGoogleアカウントと紐付いているため自動的にChromeの環境と同期が行われすぐに使い始めることができます。

以上でCloudReadyの導入が完了です。

## 詰まったところと解決方法

### インターネットに繋がらない

私はPCを有線接続（イーサネット）で接続していたのですが、CloudReadyの初期設定で上手く接続ができませんでした。
ChromeOSの初期設定をすすめる上でインターネット接続ができないと先に進めずつまずいたのですが、仮想マシンの設定の編集からネットワークアダプタを選択し。

![ブリッジ接続からNATへ変更する様子](/images/try-cloudready/image10.png)

**デフォルトのブリッジからNATに変更**することですんなりと繋がるようになりました。
繋がらない場合はお使いの環境に合わせてデフォルトから適宜設定を変えてみてください。

### ロック解除に毎回パスワードを打つのが大変

デフォルトではGoogleのパスワードを利用してロックを解除するのですが、私はGoogleのログインのパスワードに長い文字列を設定していたためロックのたびに入力するのが、がかなりストレスでした。設定 → ユーザー → 画面ロックで画面ロックの解除方法をパスワードから6ケタのPINに変更できます。

![PINを設定する様子](/images/try-cloudready/image11.png)

PINにしてからストレスなくロックの解除ができるようになりました。

:::message
ロック解除にPINを設定しても初回起動時のログインではパスワード入力が必須になります。
:::

## Linuxが起動できない問題

CloudReady（ChromeOS）にはターミナル機能とLinuxの機能が搭載されています。
ターミナルは標準で搭載されており、`Ctrl`＋`Shift`＋`t`で呼び出すことができます。

![CloudReadyのターミナル画像](/images/try-cloudready/image11.png)

それと合わせて使用できるLinux機能が存在し、設定からインストールできるはずなのですが...。

![Linuxが起動できない様子](/images/try-cloudready/image12.png)

しかし仮想マシンで実行しているCloudReadyでは設定画面に項目が存在するものの起動ができませんでした。

所持している別のPCにメインOSとしてインストールし実行したときはLinuxとターミナルを普通に起動できたため仮想マシンでのサポートがされていないのではないかと感じました。

> Neverware never recommends running CloudReady as a VM for production use cases as the security and management benefits are reduced or eliminated when a host-OS is also involved.
> （公式ページから引用）

> Neverwareは、ホストOSも関与している場合、セキュリティと管理のメリットが減少または排除されるため、本番ユースケースのVMとしてCloudReadyを実行することを推奨しません。

公式のVMwareイメージ配布ページでもこのように説明がされている通り、やはり仮想マシンではLinuxの実行は現実的で無いような気がします。
ChromeOSで開発環境を構築している情報も多く見受けられますが、CloudReady開発マシンとして運用する場合は仮想マシンではなく、メインのOSとして正規にインストールするしかないみたいです。

上記を踏まえても、仮想マシンでの運用は動作確認と検証用に留めるのが理想的だと感じました。
CloudReadyを触っていく中でこの部分についても引き続き検証していきたいです。

## おわりに

仮想マシンで手軽にChromeOSの環境を動作させられるのが便利だと感じました。
今後ChromeOSがCloudReadyに買収されるなどChromeOSが今後どんどん使いやすくなっていくと思われます。

今回紹介した方法はあくまで検証用や動作確認用ですが、手元にWindowsPCが1台あれば環境を大きく変えることなく試すことができるのでChromeOSに触ってみたい方には最適な手段だと感じました。今回紹介しているのは仮想マシンでの利用ですが、CloudReady側としてはPCにメインのOSとしてインストールするのを推奨しているため今度は、メインOSとして使ってみるのも面白そうです。

昨今はブラウザベースで利用できるサービスも多くあります。Notionを使ったドキュメント編集などのブラウザベースの作業をするのに困ることはなさそうです。

この機会にCloudReadyを使ってChromeOSデビューをしてみるのはいかがでしょうか。

最後まで読んでいただきありがとうございました。

## 参考

- [Chromium OS 派生 CloudReady - なろぐ 2](https://narolll.hateblo.jp/entry/20201027/1603784651)
- [グーグル、「Chromium」ベースの OS「CloudReady」の Neverware 買収 - ZDNet Japan](https://japan.zdnet.com/article/35164052)
