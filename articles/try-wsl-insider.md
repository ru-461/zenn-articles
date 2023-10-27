---
title: "Macに入れたWindows ARM64 InsiderPreview上でWSL2を動かそうと試みた話"
emoji: "🐴"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["windows", "mac", "wsl2", "parallels", "arm"]
published: true
---

## はじめに

Mac上でWindowsを動かすといったことがIntel製CPUを搭載していたMacでは[Boot Camp](https://support.apple.com/ja-jp/HT201468)や[Parallels Desktop](https://www.parallels.com/jp)を使うことで容易に実現できていました。しかし、2020年秋に登場したMacシリーズでCPUがAppleSiliconへと変わりアーキテクチャがarm64に変わり。その影響でWindowsをMac上で動かすのが困難となっていましたが、先日Parallels Desktop 16がバージョン16.5にて**AppleSilicon に対応**[^1]しました。これでParallels DesktopはIntel Mac ・ AppleSilicon Mac両方へ対応したことになります。

[^1]: [M1 および Intel チップセット双方をサポートした Parallels Desktop 16.5 for Mac を発表. 最新 Mac 上で Windows 10 をネイティブ スピードで稼働でき、 シームレスなユーザー エクスペリエンスに数百万のユーザーが支持¹](https://www.parallels.com/jp/news/press-releases/show/2021-pd16-5-m1chip)

現在、AppleSiliconのMacでPreview版Windows10が動作する段階まで来ています。まだまだ一部の環境でしか使用されていないarm64版Windows10ですが、Previewビルドにて**x64 アプリのエミュレーションが実現**[^2]するなど着々と進化を遂げております。

[^2]: [Introducing x64 emulation in preview for Windows 10 on ARM PCs to the Windows Insider Program | Windows Insider Blog](https://blogs.windows.com/windows-insider/2020/12/10/introducing-x64-emulation-in-preview-for-windows-10-on-arm-pcs-to-the-windows-insider-program)

一般的にライセンスを購入して使用できるようになるかはまだ定かではありませんが、現状arm Preview版のWindows10でWSL2をどこまで使えるのかを試してみました。

結論から述べると、現状の環境ではWSL1は起動しますが、WSL2は**起動させることができませんでした**。今回は検証作業の中でいくつか気になることや気付きがあったため記事として共有します。

## 検証環境

- macOS BigSurバージョン11.3（ホストOS）
- Parallels Desktop 16 for Macバージョン16.5.0
- Windows 10 on ARM Insider Preview OSビルド21354.1（ゲストOS）

## 事前準備

### Preview arm版Windows10の入手

Windows Insider Programに参加することで以下のページからPreview版のビルドを無料でダウンロードできます。

https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewARM64?wa=wsignin1.0

入手したWindowsイメージはVHDXファイルとなっており、Parallels Destopにて選択することで起動させることができます。

ParallelsDesktopは以下のページからダウンロードしてインストールできます。

https://www.parallels.com/jp/products/desktop/trial/

ParallelsDesktopは有料のソフトですが、**無料で14日間のトライアル版を使用できます**。永久ライセンスはバージョンごとに買い切りとなっておりバージョンアップ時に買い直す必要があります。また現在はサブスクリプション形式でのライセンス購入もできるようです。

### バージョン情報を確認

Mac上で仮想化したWindows10から見える情報はこのようになっております。

![Windows10のバージョン情報](https://storage.googleapis.com/zenn-user-upload/7yj9wv42q532az3nlffiwc8kg237)

プロセッサとしてMac側のAppleSiliconが認識されており、問題なく動作しております。実装RAMは仮想マシンの設定から、4GBを割り当てておりしっかりと認識されています。Windows10 ProバージョンDevとして動作します。

全体的にネイティブに動作しているWindows10と比較するとアニメーションの描画時など少しもっさりするかなといった部分はありますが、基本的な操作は問題なく行えるくらいに快適に動作してます。

この環境にWSL2（Windows Subsystems Linux 2）を導入して動かせるか試していきます。

## WSL2を導入する

WSLのインストールは最新のWindows Previewにおいてコマンド1つでインストールが可能になりました。

WSLの公式のドキュメントに日本語で詳しく説明があります。

https://docs.microsoft.com/ja-jp/windows/wsl/install-win10

WSL2を有効にするためには、仮想マシン周りの設定変更やWSL2のデフォルト化などの手順を踏まなければなりませんが、今後ワンライナーにより簡単にWSL2の環境が作れるようになるようです。便利ですね。

今回は、簡略化されたコマンド`wsl --install`を実際に使用して環境を構築していきます。

### WSL2の有効化

Parallels DesktopでWindowsが起動したら管理者権限で起動したコマンドプロンプトを起動して以下のコマンドを実行します。

```shell:コマンドプロンプト
> wsl --install
```

これでWSL2が有効となりデフォルトとして設定されます。再起動後に有効化されるので、ここで一旦Windows10を再起動します。

再起動後、wslコマンドが有効になります。以下のコマンドを入力すると簡単なコマンドのヘルプが表示されます。

```shell:コマンドプロンプト
> wsl --help
```

これでWSL2が有効となり、ディストリビューションをインストールする準備が整いました。

### WSL2にディストリビューションを追加する

WSLを有効にしただけではディストリビューションがインストールされていません。

```shell:コマンドプロンプト
# インストールされているディストリビューションをリスト表示
> wsl --list
Linux 用 Windows サブシステムには、ディストリビューションがインストールされていません。
ディストリビューションは Microsoft Store にアクセスしてインストールすることができます:
https://aka.ms/wslstore
```

今回は、UbuntuをWSL2のディストリビューションとしてインストールしてみます。以下のコマンドでインストールできるディストリビューションを表示しオプション`-d`を指定してインストールします。

```shell:コマンドプロンプト
> wsl --list --online
インストールできる有効なディストリビューションの一覧を次に示します。
 'wsl --install -d <Distro>'を使用してインストールします。

NAME            FRIENDLY NAME
Ubuntu          Ubuntu
Ubuntu-18.04    Ubuntu 18.04 LTS
Ubuntu-20.04    Ubuntu 20.04 LTS

# Ubuntu-20.04 LTS をインストール
> wsl --install -d Ubuntu-20.04
```

Ubuntu 20.04 LTSのインストールが開始されます。インストールが完了すると自動的に起動します。

### 起動する

WSL2にUbuntu 20.04 LTSがインストールできたので起動してみます。

起動まではうまくいきましたが、エラー表示がでてここから先に進みませんでした。

![WSL2起動エラーの画像](/images/try-wsl-insider/image01.png)
*一部文字化けしているがエラーのよう*

### エラー解決に向けて試したこと

このままエラーで引き下がれないので、どうにか起動できないか試行錯誤してみました。

よくあるエラーとして、カーネルの更新がされていないというのがあるようです。そこで半信半疑ながらに公式のガイドを参考にカーネルの更新をしてみます。`wsl --install`を実行したときにカーネルは更新されているのですが、念の為もう一度更新をインストールしてみます。

:::message
カーネルの更新プログラムパッケージはx64向けとARM64向けで分かれています。今回使用しているのはARM64アーキテクチャのためARM64用のカーネル更新プログラムパッケージを選択しインストールします。
:::

[こちらのページ中段](https://docs.microsoft.com/ja-jp/windows/wsl/install-win10)にあるダウンロードリンクからダウンロードしてインストールしてみるもエラー内容は変わらず...。

そこでエラーメッセージについて調べてみるとどうやら内部仮想化の部分が有効になっていない模様です。

【参考記事】
https://sugamasao.hatenablog.com/entry/2020/06/09/090000

Paralles Desktopでは**設定から「ネスト化された仮想化を有効にする」という項目を有効にする必要があるようです**。

現在、Paralles Desktopの通常版（Standard Edition）を使用しているのですが、設定項目を探してみても該当項目は見当りませんでした。

![Standard Editionでの設定表示](/images/try-wsl-insider/image02.png)
*参考記事を見るとここに項目があるはずだが...*

少し古い情報となりますが、ネスト化された仮想化サポートはPro Editionでのみのサポートという情報があったため、勢いでPro Editionに変更して検証してみます。

こちらがPro Editionへアップグレードした後の設定画面になります。

![アップグレード後の設定画面](/images/try-wsl-insider/image03.png)
*オプションが現れず...*

アップグレード前と比較して変わったところは、メモリの上限を拡張できるようになったことだけでネスト化された仮想化を有効にする設定は現れませんでした。

ゲストOS側のBIOS設定やHyper-Vを見直したりしたのですが、仮想化に関する設定は見当たらず、現状この環境にて仮想化を有効にするのは厳しそうといった状況です。

以前のバージョンではWSL2の動作が仮想化されたWindows10上にて実現できている例が見られたので、今後も引き続き検証していきたいところです。

### 補足 : WSL1 として設定する

本題はWSL2の実現なのですが、この環境でWSL1が動作することを検証中に確認できたので補足としてまとめます。

`wsl --install`の実行直後はWSL2がデフォルトとして設定されているため、以下のコマンドでWSL1をデフォルトのバージョンとして設定することでをWSL1として起動できます。

```shell:コマンドプロンプト
# バージョン1をデフォルトにする
> wsl --set-default-version 1
```

この状態でディストリビューションをインストールして起動することでWSL1として設定できます。

![WSL1として設定した様子](/images/try-wsl-insider/image04.png)

WSL1とWSL2の違いは以下のページでまとめられています。

https://docs.microsoft.com/ja-jp/windows/wsl/compare-versions

## おわりに

今回、WSL2をParallels Desktopに導入したWindows10 arm64 Insider Preview上で有効化できるのかを検証しました。結果としてはうまく動作させることができませんでした。まだWindows10がPreviewビルドであったり、Parallels DesktopがAppleSiliconに対応したばかりだったりとまだまだ技術面で課題があるように感じました。

本題からはそれるのですが、現在Ubuntu Desktopのarm版が[Ubuntu 20.04.2.0 LTS (Focal Fossa) Daily Build](https://cdimage.ubuntu.com/focal/daily-live/current)として公開されています。このイメージをParallels Desktopに導入することで仮想化したUbuntu DesktopをAppleSilicon上で使うことができます。こちらは問題なく動作検証できました。

需要がどのくらいあるかは未知数ですが、arm版のWindows10がMacbook上で快適に動作するようになったなどと興味深い結果が得られたので今後のアップデートにも注目していきたいところです。

最後まで読んでいただきありがとうございました。

## 参考

- [M1 Mac正式対応の「Parallels Desktop 16.5」提供開始 - PC Watch](https://pc.watch.impress.co.jp/docs/news/1318735.html)
- [M1搭載MacBook ProでWindowsアプリを動かす｢Parallels Desktop｣を試す | Business Insider Japan](https://www.businessinsider.jp/post-234408)
- [ParallelsでWindows 10のWSL2を使う - すがブロ](https://sugamasao.hatenablog.com/entry/2020/06/09/090000)
