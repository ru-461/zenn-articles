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

そこで実際にVoltaをWindowsにインストールしてZenn公式のNode.js製ツールZenn Editorを導入して執筆環境を構築するところまでやってみました。

# インストール

今回はWindows10にVoltaを使用してNode.jsのLTSをインストールしてZenn Editorをブラウザで動かせるところまで進めていきます。WSL2でも同じことができますが、クロスプラットフォームを体験するためにあえてWindows10で実装していきます。

## インストーラーでVoltaをインストール

Voltaの公式サイトからWindows用インストーラーを入手してVoltaをWindowsへインストールしていきます。
ダウンロードしたインストーラー（volta-version-windows-x86_64.msi）をダブルクリックすることでインストールウィザードが起動します。ダウンロードファイル名のversionsにはインストールされるバージョン名が入ります。

![Voltaインストールウィザードの画像](/images/win-zennenv-with-volta/image02.png)

「Next」をクリックしてインストールを進めます。
Voltaをインストールする際に以下のようなUAC（ユーザーアカウント制御）が表示されます。

![ユーザーアカウント制御の画像](/images/win-zennenv-with-volta/image03.png)

許可を求められたら「はい」をクリックして続行します。
これでインストールが完了します。WindowsであってもVoltaのインストールもかなり高速で終わります。

## コマンドの確認

Voltaのインストールが完了し、ウィザードを終了した時点でパスが自動的に追加されVoltaコマンドが使えるようになります。

コマンドを確認するためにWindows標準のコマンドプロンプトを立ち上げて確認してみます。

`Volta`と入力すると以下のようなコマンド一覧が表示されます。

![Voltaコマンドの確認画像](/images/win-zennenv-with-volta/image04.png)

コマンドが実行できることを確認できれば、WindowsへのVoltaのインストールは成功しています。これでVoltaをWindowsで使用する準備が整いました。

## VoltaからNode.jsをインストール

VoltaはあくまでNode.jsのバージョン管理マネージャのため、デフォルトとなるNode.jsのバージョンを設定する必要があります。今回は記事執筆時点でLTSリリースとなっているバージョンをインストールしていきます。

```shell
volta install node
```

とすることで自動的に現在のLTSリリースの最新版が選択されインストール、デフォルトとして設定されます。

Voltaを経由でインストールしたNode.js、ツールチェインは`volta list`で一覧表示できます。

```shell
volta list
⚡️ Currently active tools:
    Node: v14.17.3 (default)
```

上の例だと**Node.js v14.17.3**がデフォルトとして使用されるという意味になります。また同じようにnpmやYarnといったパッケージマネージャのインストール、デフォルト化もできます。

# Zenn執筆環境の構築

Zennには執筆環境＆記事プレビューツールとしてNode.js製の[Zenn Editor（Zenn CLI）](https://github.com/zenn-dev/zenn-editor)があります。

Zenn公式の投稿にインストールから記事の投稿までの流れを説明している記事があるためこちらを参考に進めます。

https://zenn.dev/zenn/articles/install-zenn-cli

## Zenn CLIの導入

まず、記事を管理するためのディレクトリを作成する必要があるため、空のディレクトリを作成します。今回はユーザーディレクトリのドキュメントルート（C¥Users¥user¥Documents）に記事管理用ディレクトリを作成しました。WindowsではUNIXと同じように`mkdir`とすることでディレクトリを作成できます。

```shell
# ドキュメントルートへ移動
cd C¥Users¥user¥Documents

# Zennの投稿管理用ディレクトリを作成
mkdir zenn-contents
```

続いて作成したnpmプロジェクトの初期化を行いZenn CLIのインストールを行います。

```shell
# 記事管理ディレクトリに移動
cd zenn-contents

# npmプロジェクトの初期化
npm init --yes

# Zenn CLIをプロジェクトにインストール
npm install zenn-cli
```

上の流れで作成したディレクトリにZenn Cliのインストールが完了します。

## Zennのセットアップ

Zennの記事執筆に必要なディレクトリとファイル群を以下のコマンドで生成します。

```shell
# Zennのプロジェクトを初期化
npx zenn init
```

コマンド実行後のディレクト構成は以下のようになります。

![エクスプローラーから見る構成画像](/images/win-zennenv-with-volta/image05.png)
***エクスプローラーで表示した様子***

articlesディレクトリbooksディレクトリが作成されました。


# おわりに