---
title: "Voltaを使ってWindowsにZennの執筆環境を構築する"
emoji: "🦁"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["windows","volta","zenncli"]
published: false
---

# はじめに

Rust製のコマンドやユーティリティをよく見るようになりました。Rustの特徴として**パフォーマンス・信頼性・生産性**に優れているというものがあり、多くのRust製ツールがクロスプラットフォームに動作するのが魅力の1つになっています。

近年注目されているRust製ツールのひとつに「Volta」いうJavaScriptツールマネージャがあります。

https://volta.sh/

Rust製で高速に動作することはもちろんのこと、クロスプラットフォームでの動作もできるバージョンマネージャとなっています。

nodebrewやnodenvなどNode.jsバージョンマネージャは数多くありますが、**Voltaの特徴的な点はUNIXだけではなくWindows向けのインストール手段が提供されていることにあります**。Voltaの公式サイトにはインストールガイドの中にWindows向けのインストーラー（.msi）があり、常に最新版が提供され続けています。

![Windows用インストールガイドの画像](/images/win-zennenv-with-volta/image01.png)

そこで実際にVoltaをWindowsにインストールしてZenn公式のNode.js製ツールZenn Editorを導入して執筆環境を構築するところまでやってみました。

# インストール

今回はWindows10にVoltaを使用してNode.jsのLTSをインストールしてZenn Editorをブラウザで動かせるところまで進めていきます。WSL2でも同じことができますが、クロスプラットフォームを体験するためにあえてWindows10で実装していきます。環境に依存しない環境構築という点に重点をおいて進めていきます。

## インストーラーでVoltaをインストール

Voltaの公式サイトからWindows用インストーラーを入手してVoltaをWindowsへインストールしていきます。
ダウンロードしたインストーラーファイル（volta-version-windows-x86_64.msi）をダブルクリックすることでインストールウィザードが起動します。ダウンロードファイル名のversionsにはインストールされるバージョン名が入ります。

![Voltaインストールウィザードの画像](/images/win-zennenv-with-volta/image02.png)

「Next」をクリックしてインストールを進めます。
Voltaをインストールする際に以下のようなUAC（ユーザーアカウント制御）が表示されます。

![ユーザーアカウント制御の画像](/images/win-zennenv-with-volta/image03.png)

許可を求められたら「はい」をクリックして続行します。
これでWindowsへのVoltaのインストールが完了します。

## コマンドの確認

Voltaのインストールが完了し、ウィザードを終了した時点でパスが自動的に追加されVoltaコマンドが使えるようになります。

コマンドを確認するためにWindows標準のコマンドプロンプトを立ち上げて確認してみます。

`Volta`と入力すると以下のようなコマンド一覧が表示されます。

![Voltaコマンドの確認画像](/images/win-zennenv-with-volta/image04.png)

コマンドが実行できることを確認できれば、WindowsへのVoltaのインストールは成功しています。これでVoltaをWindowsで使用する準備が整いました。

Voltaで使用できるコマンドについては公式リファレンス[Volta Commands | Volta](https://docs.volta.sh/reference/)にわかりやすくまとめられています。`volta`でもコマンドの使用例を確認できますが、詳しい解説については公式リファレンスも合わせて読むと理解しやすいです。

## VoltaからNode.jsをインストール

VoltaはあくまでNode.jsのバージョン管理マネージャのため、デフォルトとなるNode.jsのバージョンを設定する必要があります。今回は記事執筆時点でLTSリリースとなっているバージョンをインストールしていきます。

```shell
volta install node
```

とすることで自動的に**安定版になっているLTSリリースの最新版**が選択されインストール、**デフォルト**として設定されます。

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

まず、記事を管理するためのディレクトリを作成する必要があるため、空のディレクトリを作成します。今回はユーザーディレクトリのドキュメントルート（C¥Users¥user¥Documents）に記事管理用ディレクトリを作成しました。

:::message
WindowsではUNIXと同じように`mkdir`とすることでディレクトリを作成できます。
:::

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

上の流れで作成したディレクトリにZenn CLIのインストールが完了します。

## Zennのセットアップ

Zennの記事執筆に必要なディレクトリとファイル群を以下のコマンドで生成します。

```shell
# Zennのプロジェクトを初期化
npx zenn init
```

コマンド実行後のディレクト構成は以下のようになります。

![エクスプローラーから見る構成画像](/images/win-zennenv-with-volta/image05.png)
***エクスプローラーで表示した様子***

`npm init --yes`で自動生成されたpackage.jsonのあるディレクトリ内にarticlesディレクトリ・booksディレクトリ、その他必要なファイルがまとめて作成されました。

## 記事の作成とプレビュー

Zennの記事執筆環境が完成したので実際に記事を生成してプレビューしてみます。記事管理のディレクトリ内で`npx zenn preview`とするとサーバーが起動し、[Zenn Editor（localhost:8000）](http://localhost:8000/)にて記事をプレビューできるようになります。

```shell
# 記事をプレビュー
npx zenn preview
```

![記事のプレビュー画像](/images/win-zennenv-with-volta/image06.png)

記事のプレビューページでは作成した記事をZennでの表示で確認できます。記事を書く際にローカルで執筆してリアルタイムにZennでどのように見えるかを確認できるのが便利です。

このプレビュー機能の嬉しいポイントとしてホットリロードを使用してのライブプレビューができるというものがあります。Windowsの環境でも実際にファイルを作成してプレビューしてみます。

`npx zenn new:article`で新規に記事を作成するとリアルタイムにライブプレビューへ反映されます。

![Windowsで見るライブプレビュー機能の画像](/images/win-zennenv-with-volta/image07.png)
***新規作成した記事をプレビューしている様子***

上の画像ではWindows標準のメモ帳でマークダウンファイルを編集している様子です。右のメモ帳で編集し上書き保存すると、左に並べて表示したZenn Editorでリアルタイムにマークダウンを更新表示できています。実際に記事を書くときにメモ帳は少し使いにくいので実際はVSCodeなどを使うがおすすめです。VSCodeがインストールされている場合は、記事を管理しているディレクトリ内で`code .`と実行することでVSCodeにて直接ディレクトリを開くことができます。

ここまでWindowsの環境でも問題なくVoltaを使用してのNode.js実行環境の構築、記事の作成・編集まで行うことが確認できました。

実際にZennへ記事をデプロイするときはGitHubリポジトリ連携でデプロイ可能です。本記事ではGitのセットアップやリポジトリの設定は割愛します。

https://zenn.dev/zenn/articles/connect-to-github

GitHub連携するためにGitがローカルで使用できる環境が必要となります。Windowsでは[Git for Windows](https://gitforwindows.org/)が使用できるため、Windowsのローカル環境だけで本格的に記事を書いていくことができます。

GitHubでリポジトリ連携して自動デプロイできる環境があるおかげで環境が変わってもVSCodeの設定同期とリポジトリのクローンだけで記事をローカルで執筆し続けられるのがメリットです。WSL2やMacなどに合わせてWindowsでも同じように執筆できるため執筆できる幅が広がるように感じます。

# おわりに

今回はWindows上で`Volta`を使用してZennの記事執筆環境を構築してみました。Rust製のツールということもあり、高速な動作と安定性がありWindowsであっても問題なく動作するのが良かったです。また**クロスプラットフォーム対応でOSを問わず使える**のがユーザー目線で見たときに利便性を感じられるいい点だと感じました。**コマンドの使用方法や使い勝手も変わらないため、環境が変わっても同じ感覚で使い続けられるのは大きなメリット**に感じます。

最後まで読んでいただきありがとうございました。