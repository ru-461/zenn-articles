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

Rust製で高速に動作することはもちろんのこと、クロスプラットフォームでの動作もできるNode.jsのバージョン管理ツールとなっています。Voltaの公式サイトにはインストールガイドの中にWindows向けのインストーラー（.msi）があります。そこで実際にVoltaをWindowsにインストールしてZennの執筆環境（ZennEditor）を構築するところまでやってみました。

# インストール

今回はWindows10にVoltaを使用してNode.jsのLTSをインストールしてZennEditorをブラウザで動かせるところまで進めていきます。WSL2でも同じことができますが、クロスプラットフォームを体験するためにあえてWindows10で実装していきます。

# おわりに