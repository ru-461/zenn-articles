---
title: "Windows上でChromeOSを動かす [CloudReady]"
emoji: "☁"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["windows"]
published: false
---

# はじめに

ChromeBook には Google ChromeOS という OS が搭載されています。
これはオープンソースである ChromiumOS をベースに Google が設計した OS で、現在 Chromebook にて採用されています。

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

# 仮想マシンの用意

まず ChromeOS(CloudReady)を Windows 上で動作させるために仮想マシンを用意します。
仮想マシンを利用することで Windows の環境を汚すことなく、OS を動かすことが可能となります。

CloudReady を仮想マシンで利用する場合は VMware Workstation Player での動作のみサポートされているため VMware Workstation Player を使用します。VirtualBox では上手く動作しない模様です。

VMware Workstation Player は以下のページからダウンロード可能です。

https://www.vmware.com/jp/products/workstation-player/workstation-player-evaluation.html

![](https://storage.googleapis.com/zenn-user-upload/w0r475gfqn6tcd0zvte1g9qr21a8)

VMware Workstation Player for Windows をインストールします。

![](https://storage.googleapis.com/zenn-user-upload/lik7ygtirmi2gvhvvljiwt079r48)

この画面が表示されれば VMware のインストール完了です。


# おわりに
