---
title: "Macに入れたWindows ARM64 InsiderPreview上でWSL2を動かしてみようと試みた話"
emoji: "🐴"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["windows","mac","wsl","parallels","arm"]
published: false
---

# はじめに

Mac 上で Windows を動かすといったことが以前の Intel 製 CPU を搭載していた Mac では [Boot Camp](https://support.apple.com/ja-jp/HT201468) や [Parallels Desktop](https://www.parallels.com/jp/) といったツールを使うことで容易に実現できていましたが 2020 年秋に登場した Mac シリーズで CPU が AppleSilicon に変わりそれとともにアーキテクチャが arm64 に変わりました。その影響で Windows を Mac 上で動かすのが困難となっていましたが、先日 Parallels Desktop 16 がバージョン 16.5 にて正式に AppleSilicon に対応[^1]し Windows が動くようになりました。これで Parallels Desktop は Intel Mac ・ AppleSilicon Mac 両方へ対応したことになります。

[^1]: [M1 および Intel チップセット双方をサポートした Parallels Desktop 16.5 for Mac を発表. 最新 Mac 上で Windows 10 をネイティブ スピードで稼働でき、 シームレスなユーザー エクスペリエンスに数百万のユーザーが支持¹](https://www.parallels.com/jp/news/press-releases/show/2021-pd16-5-m1chip/)

ちょうど、Windows も arm64 Insider Preview を Windows InsidersProgram で配布しており AppleSilicon の Mac で Preview 版の Windows が動作する段階まで来ています。

arm 版 Windows が一般的にライセンスを購入して使用できるようになるかはまだ定かではありませんが、現状 Preview 版の Windows で WSL2 をどこまで使えるのかを試してみました。

結論から話すと、現状の環境では `起動させることができませんでした` 。しかし検証作業の中でいくつか気になることや気付きがあったため記事として共有します。

# 検証環境

- macOS BigSur 11.3 (ホスト OS)
- Parallels Desktop 16 for Mac バージョン 16.5.0
- Windows 10 on ARM Insider Preview OS ビルド 21354.1 (ゲスト OS)

# 事前準備

## Preview arm 版Windows10の入手

Windows Insider Program に参加することで以下のページから Preview 版のビルドを無料でダウンロードできます。

https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewARM64?wa=wsignin1.0

入手した Windows イメージは `VHDX 形式`となっており、Parallels Destop にて選択することで起動させることができます。

ParallelsDesktop は以下のページからダウンロードしてインストールできます。

https://www.parallels.com/jp/products/desktop/trial/

ParallelsDesktop は有料のソフトですが、無料で ` 14 日間のトライアル版` を使用できます。永久ライセンスはバージョンごとに買い切りとなっておりバージョンアップ時に買い直す必要があります。また現在はサブスクリプション形式でのライセンス購入もできるようです。

## バージョン情報を確認

Mac 上で仮想化した Windows10 から見た情報はこのようになっております。

![Windowsのバージョン情報](https://storage.googleapis.com/zenn-user-upload/7yj9wv42q532az3nlffiwc8kg237)

プロセッサとして Mac 側の AppleSilicon が認識されており、問題なく動作しております。実装 RAM は仮想マシンの設定から、4GB を割り当てておりしっかりと認識されています。Windows Pro バージョン Dev として動作します。

全体的な動作としては、ネイティブに動作している Windows10 と比較するとアニメーションの描画時など少しもっさりするかなといった部分はありますが、基本的な操作は問題なく行えるくらいに快適に動作してます。

この環境に WSL2(Windows Subsystems Linux2)を導入して動かせるか試していきます。

# WSL2を導入する

WSL のインストールは最新の Windows Preview において `コマンド 1 つ` でインストールが可能になりました。

公式のドキュメントに日本語で詳しく説明があります。

https://docs.microsoft.com/ja-jp/windows/wsl/install-win10

現在 WSL2 を有効にするためには、仮想マシン周りの設定変更や WSL2 の既定化などの手順を踏まなければなりませんが、今後それらの手順がワンライナーに置き換えられより簡単に WSL2 の環境が作れるようになるようです。便利ですね。

今回は、簡略化されたコマンド `wsl --install` を実際に使用して環境を構築していきます。
## WSL2の有効化

Parallels Desktop で Windows が起動したら管理者権限で起動したコマンドプロンプトを起動して以下のコマンドを実行します。

```shell:コマンドプロンプト
> wsl --install
```

これで WSL2 が有効となり既定として設定されます。再起動後に有効化されるので、ここで一旦 Windows を再起動します。

再起動後、wsl コマンドが有効になります。以下のコマンドを入力すると簡単なコマンドのヘルプが表示されます。

```shell:コマンドプロンプト
> wsl --help
```

これで WSL2 が有効となり、ディストリビューションをインストールする準備が整いました。

## WSL2にディストリビューションを追加する

WSL を有効にしただけではディストリビューションがインストールされていません。

```shell:コマンドプロンプト
# インストールされているディストリビューションをリスト表示
> wsl --list
Linux 用 Windows サブシステムには、ディストリビューションがインストールされていません。
ディストリビューションは Microsoft Store にアクセスしてインストールすることができます:
https://aka.ms/wslstore
```

今回は、Ubuntu を WSL2 のディストリビューションとしてインストールしてみます。以下のコマンドでインストールできるディストリビューションを表示し `-dオプション` を指定してインストールします。

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

Ubuntu 20.04 LTS のインストールが開始されます。インストールが完了すると自動的に起動します。

## 起動する

WSL2 に Ubuntu 20.04 LTS がインストールできたので起草してみます。

起動まではうまくいきましたが、エラー表示がでてここから先に進みませんでした。

![WSL2起動エラーの画像](https://storage.googleapis.com/zenn-user-upload/oshvlzztlom0cnxjz7v3kc29qta0)
***一部文字化けしている***

## 試したこと

このままエラーで引き下がれないので、どうにか起動できないか試行錯誤してみました。

よくあるエラーとして、カーネルの更新がされていないというのがあるようです。そこで半信半疑ながらに公式のガイドを参考にカーネルの更新をしてみます。`wsl --install`コマンドを実行したときにカーネルは更新されているのですが、念の為もう一度更新をインストールしてみます。

:::message
カーネルの更新プログラムパッケージは x64 向けと ARM64 向けで分かれています。今回使用しているのは ARM64 アーキテクチャのため ARM64 用のカーネル更新プログラムパッケージを選択しインストールします。
:::

[こちらのページ中段](https://docs.microsoft.com/ja-jp/windows/wsl/install-win10)にあるダウンロードリンクからダウンロードしてインストールしてみるもエラー内容は変わらず...。

そこでエラーメッセージについて調べてみるとどうやら内部仮想化の部分が有効になっていない模様です。

【参考記事】
https://sugamasao.hatenablog.com/entry/2020/06/09/090000

Paralles Desktop では設定から `「ネスト化された仮想化を有効にする」` という項目を有効にする必要があるようです。

現在、Paralles Desktop の通常版(Standard Edition)を使用しているのですが、設定項目を探してみても該当項目は見当りませんでした。

![Standard Editionでの設定表示](https://storage.googleapis.com/zenn-user-upload/8mbsbqfe6yp54ocs3c3oq7pdgp2b)
***参考記事を見るとここに項目があるはずだが...***

https://download.parallels.com/desktop/v12/docs/ja_JP/Parallels%20Desktop%20User's%20Guide/37830.htm

少し古い情報となりますが、ネスト化された仮想化サポートは Pro Edition でのみのサポートという情報があったため、勢いで Pro Edition に変更して検証してみます。

こちらが Pro Edition へアップグレードした後の設定画面になります。

![アップグレード後の設定画面](https://storage.googleapis.com/zenn-user-upload/0hazekic80sefdfsrstc67z0plp0)

アップグレード前と比較して変わったところは、メモリの上限を拡張できるようになったことだけでネスト化された仮想化を有効にする設定は現れませんでした。

以前のバージョンでは WSL2 の動作が仮想化された Windows 上にて実現できている例が見られたので、今後実現できるようになってほしいです。

# おわりに

今回、WSL2 を Parallels Desktop に導入した Windows arm64 Insider Preview 上で有効化できるのかを検証しました。結果としてはうまく動作させることができませんでした。まだ Windows が Preview ビルドであったり、Parallels Desktop が AppleSilicon に対応したてだったりと技術面で課題があるように感じました。

本題からはそれるのですが、現在 Ubuntu Desktop の arm 版が[Ubuntu 20.04.2.0 LTS (Focal Fossa) Daily Build](https://cdimage.ubuntu.com/focal/daily-live/current/)として公開されています。このイメージを Parallels Desktop に導入することで仮想化した Ubuntu Desktop を AppleSilicon 上で使うことができます。こちらは問題なく動作検証できました。

仮想化された環境の中で仮想化技術を使うという需要がどのくらいあるかはわかりませんが、arm 版の Windows10 が Macbook 上で快適に動作するようになったなどと興味深い結果が得られたので今後の動向にも注目していきたいところです。

最後まで読んでいただきありがとうございました。


