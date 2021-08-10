---
title: "Voltaを使ってWindowsにZennの執筆環境を構築する"
emoji: "🦁"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["windows","volta","zenncli"]
published: false
---

# はじめに

近年、Rust製のコマンドやユーティリティをよく見るようになりました。Rust製のツールの特徴として高速であり、クロスプラットフォームに対応できるというものがあります。

Rust製で作られたVoltaというNode.jsバージョンマネージャがあります。

https://volta.sh/

Rust製で高速に動作することはもちろんのこと、クロスプラットフォームでの動作もできるNode.jsのバージョン管理ツールとなっています。Voltaの公式サイトにはインストールガイドの中にWindows向けのインストーラー（.msi）があります。

![Windows用インストールガイドの画像](/images/win-zennenv-with-volta/image01.png)

そこで実際にVoltaをWindowsにインストールしてZennの執筆環境（Zenn Editor）を構築するところまでやってみました。

# インストール

今回はWindows10にVoltaを使用してNode.jsのLTSをインストールしてZennEditorをブラウザで動かせるところまで進めていきます。WSL2でも同じことができますが、クロスプラットフォームを体験するためにあえてWindows10で実装していきます。

## インストーラーでVoltaをインストール

Voltaの公式サイトからWindows用のインストーラーを入手してVoltaをWindowsへインストールしていきます。
ダウンロードしたインストーラー（volta-version-windows-x86_64.msi）をダブルクリックすることでインストールウィザードが起動します。ダウンロードファイル名のversionsにはインストールされるバージョン名が入ります。

![Voltaインストールウィザードの画像](/images/win-zennenv-with-volta/image02.png)

Nextをクリックしてインストールを進めます。
Voltaをインストールする際に以下のようなUAC（ユーザーアカウント制御）が表示されます。

![ユーザーアカウント制御の画像](/images/win-zennenv-with-volta/image03.png)

許可を求められたら「はい」をクリックして続行します。
これでインストールが完了します。WindowsであってもVoltaのインストールもかなり高速で終わります。

# おわりに