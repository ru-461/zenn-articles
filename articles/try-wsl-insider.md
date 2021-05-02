---
title: "Macに入れたWindows ARM64 InsiderPreview上でWSL2を動かしてみた"
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

# 検証環境

- macOS BigSur 11.3 (ホスト OS)
- Parallels Desktop 16 for Mac バージョン 16.5.0
- Windows 10 on ARM Insider Preview OS ビルド 21354.1 (ゲスト OS)

# 事前準備

## Preview ARM 版Windowsの入手

Windows Insider Program に参加することで以下のページから Preview 版のビルドを無料でダウンロードできます。

https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewARM64?wa=wsignin1.0

入手した Windows イメージは VHDX 形式となっており、ParallelsDestop にて選択することで起動させることができます。

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

WSL のインストールは最新の Windows Preview においてコマンド 1 つでインストールが可能になりました。

公式のドキュメントに日本語で詳しく説明があります。

https://docs.microsoft.com/ja-jp/windows/wsl/install-win10

## WSL2の有効化

管理者権限で Windows 側のコマンドプロンプトを起動して以下のコマンドを実行します。

```shell:コマンドプロンプト
> wsl --install
```

これで WSL2 が有効となります。再起動後にコマンドが有効になるため、ここでゲスト OS である Windows10 を再起動します。

再起動後、wsl コマンドが有効になります。以下のコマンドを入力すると簡単なコマンドのヘルプが表示されます。

```shell:コマンドプロンプト
> wsl --help
```

これで WSL2 が有効となり、ディストリビューションをインストールする準備が整いました。

# おわりに

